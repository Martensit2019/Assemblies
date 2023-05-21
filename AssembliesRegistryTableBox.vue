<template>
  <div class="assemblies-registry-table-box">
    <div class="assemblies-registry-table-box-head">
      <Button
        id="assemblies_registry_table_box_button_excel"
        text="Экспорт в Excel"
        size="universal"
        color="primary"
        view="default"
      />
      <div class="assemblies-registry-table-box-head__btns">
        <Button
          id="assemblies_registry_table_box_button_edit"
          class="assemblies-registry-table-box-head__btn"
          text="Редактировать"
          view="smoke"
          size="small"
          disabled
        />
        <Button
          id="assemblies_registry_table_box_button_delete"
          class="assemblies-registry-table-box-head__btn"
          text="Удалить"
          view="smoke"
          size="small"
          disabled
        />
        <Button
          id="assemblies_registry_table_box_button_copy"
          class="assemblies-registry-table-box-head__btn"
          text="Копировать"
          view="smoke"
          size="small"
          disabled
        />
        <Button
          id="assemblies_registry_table_box_button_print"
          class="assemblies-registry-table-box-head__btn"
          text="Печать"
          view="smoke"
          size="small"
          @click="generatePDF"
        />
      </div>
    </div>
    <div ref="registryTable" class="assemblies-registry-table">
      <EasyDataTableCustom
        v-model:items-selected="itemsSelected"
        :headers="headers"
        :items="items"
        :theme-color="themeColor"
        hide-footer
        header-text-direction="left"
      >
        <template #item-author="item: Item">
          <router-link
            :id="`user_update_${item.id}`"
            :to="`/administration/users/${item.id}`"
            class="assemblies-registry-table__link"
          >
            {{ item.author }}
          </router-link>
        </template>
      </EasyDataTableCustom>
    </div>

    <!-- TODO: Блок assemblies-registry-table-box-footer просто вёрстка, нужно будет сделать отдельный компонент пагинации  -->

    <div class="assemblies-registry-table-box-footer">
      <div class="assemblies-registry-table-box-footer__left">
        Страница 1 / <span>32</span>
        <Select
          id="assemblies_registry_settings_pagin_page"
          v-model="pagin_page"
          placeholder="10 строк"
          class="select-page"
        />
      </div>
      <div class="assemblies-registry-table-box-footer__right">
        <ButtonIcon
          id="prev_button"
          text="Назад"
          size="small"
          icon="arrow_right"
          color="primary"
          view="smoke"
          icon-position="before"
        />
        <Button
          id="current_button"
          class="btn-active"
          text="1"
          view="smoke"
          size="small"
          disabled
        />
        <div v-for="page in pages" :key="page" class="pagination-page">
          {{ page }}
        </div>
        <ButtonIcon
          id="next_button"
          text="Вперёд"
          size="small"
          icon="arrow_left"
          color="primary"
          view="smoke"
          icon-position="after"
        />
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
  import pdfMake from 'pdfmake/build/pdfmake'
  import pdfFonts from 'pdfmake/build/vfs_fonts'
  import type { Header, Item } from 'vue3-easy-data-table'
  import { mockHeaders, mockItems } from './mockData'
  import '@/assets/styles/table.sass'
  ;(pdfMake as any).addVirtualFileSystem(pdfFonts)
  const itemsSelected = ref<Item[]>([])
  const themeColor = '#23527E'

  const headers: Header[] = mockHeaders
  const items: Item[] = mockItems
  const registryTable = ref<HTMLDivElement | null>(null)
  const pagin_page = ref()
  const pages = [2, 3, 4, 5, '...', 10]

  const generatePDF = () => {
    console.log('pdf')
    const pdfInfo = {
      pageOrientation: 'landscape',
      content: [
        {
          table: {
            widths: [40, 90, 80, 70, 70, 70, 70, 80, 70],
            body: [
              headers.map((item) => item.text),
              ...items.map((item) => Object.values(item)),
            ],
          },
        },
      ],
    }

    pdfMake.createPdf(pdfInfo).open()
  }

  onMounted(() => {
    const table = registryTable.value?.querySelector('table')
    if (table) {
      table.id = 'assemblies_registry_table'
      const header = table.querySelectorAll('th')
      if (header) {
        header.forEach((th, idx) => {
          th.id = `assemblies_registry_table_${idx}`
          const spanTh = th.querySelector('.header-text') as HTMLDivElement
          if (spanTh) spanTh.style.fontSize = '10px'
        })
      }
      const row = table.querySelectorAll('td')
      if (row) {
        row.forEach((td) => {
          td.style.fontSize = '10px'
        })
      }
    }
  })
</script>

<style scoped lang="sass">
  @import "assembliesRegistryTableBox"
</style>
