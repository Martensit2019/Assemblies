<template>
  assembliesRegistryFilter - {{ assembliesRegistryFilter }}
  <div class="assemblies-registry-table-box">
    <div class="assemblies-registry-table-box-head">
      <Button
        id="assemblies_registry_table_box_button_excel"
        text="Экспорт в Excel"
        size="universal"
        color="primary"
        view="default"
        @click="generateExcel"
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
    <div class="assemblies-registry-table__wrapper">
      <div ref="registryTable" class="assemblies-registry-table">
        <EasyDataTableCustom
          v-model:items-selected="itemsSelected"
          :headers="headers"
          :items="items"
          :theme-color="themeColor"
          hide-footer
          header-text-direction="left"
        >
          <template #item-cipher="item: Item">
            <router-link
              :id="`build_name_${item.cipher}`"
              :to="`/assemblies/view/${item.cipher}`"
              ><div class="assemblies-registry-table__link">
                {{ item.cipher }}
              </div>
            </router-link>
          </template>
          <template #item-operator="item: Item">
            <div class="assemblies-registry-table__ellipsis">
              {{ item.operator }}
            </div>
          </template>
          <template #item-author="item: Item">
            <router-link
              :id="`user_update_${item.id}`"
              :to="`/administration/users/${item.id}`"
              ><div
                class="assemblies-registry-table__link assemblies-registry-table__ellipsis"
              >
                {{ item.author }}
              </div>
            </router-link>
          </template>
          <template #item-construction_type="item: Item">
            <div class="assemblies-registry-table__ellipsis">
              {{ item.construction_type }}
            </div>
          </template>
          <template #item-comment="item: Item">
            <div class="assemblies-registry-table__ellipsis">
              {{ item.comment }}
            </div>
          </template>
          <template #item-create_at="item: Item">
            <div class="assemblies-registry-table__create_at">
              {{ formatVisualDate(item.create_at, VISUAL_DATE) || '' }}
            </div>
          </template>
        </EasyDataTableCustom>
      </div>
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
  import { getBuildListRequest } from '@/api'
  import { formatVisualDate, VISUAL_DATE } from '@/helpers/time'
  import pdfMake from 'pdfmake/build/pdfmake'
  import pdfFonts from 'pdfmake/build/vfs_fonts'
  import table2excel from 'js-table2excel'
  import type { Header, Item } from 'vue3-easy-data-table'
  import { tableHeaders } from './tableHeaders'
  import '@/assets/styles/table.sass'
  ;(pdfMake as any).addVirtualFileSystem(pdfFonts)
  const itemsSelected = ref<Item[]>([])
  const themeColor = '#23527E'

  const headers: Header[] = tableHeaders
  const items = ref<Item[]>([])
  const registryTable = ref<HTMLDivElement | null>(null)
  const pagin_page = ref()
  const pages = [2, 3, 4, 5, '...', 10]
  const fileName = `Реестр ПСМ ${new Date()}`

  const assembliesRegistryFilter: any = inject('assemblies-registry-filter')                                          <-------------------

  const generatePDF = () => {
    const tableHeaders = headers
      .map((header) => header.text)
      .reduce((prev: object[], item) => {
        prev.push({ text: item, fontSize: 10, bold: true })
        return prev
      }, [])
    if (items.value) {
      const tableRows = items.value
        .map((row) => Object.values(row))
        .map((rowItem) =>
          rowItem.reduce((prev: object[], rowText) => {
            prev.push({ text: rowText, fontSize: 10 })
            return prev
          }, [])
        )

      const pdfDoc = {
        info: {
          title: fileName,
        },
        header: [{ text: fileName }],
        content: [
          {
            table: {
              widths: [35, 73, 65, 50, 55, 40, 50, 50, 50],
              body: [tableHeaders, ...tableRows],
            },
          },
        ],
      }

      pdfMake.createPdf(pdfDoc).download(fileName)
    }
  }

  const generateExcel = () => {
    table2excel({
      column: headers.reduce((prev: object[], item) => {
        prev.push({ title: item.text, key: item.value })
        return prev
      }, []),
      data: items.value,
      excelName: fileName,
      captionName: fileName,
    })
  }

  const fetchBuildList = () => {
    getBuildListRequest().then((result) => {
      if (result.data.results) {
        items.value = result.data.results.map((build: Item) => {
          return {
            cipher: build.full_code_long,
            name: build.name,
            operator: build.doer.name,
            status: build.state.name,
            instance: build.instance || 'РЕНГА',
            author: build.author?.name,
            construction_type: build.object_type_name,
            comment: build.comment,
            create_at: build.created_at,
          }
        })
      }
    })
  }

  onMounted(() => {
    fetchBuildList()
    const table = registryTable.value?.querySelector('table')
    if (table) {
      table.id = 'assemblies_registry_table'
      const header = table.querySelectorAll('th')
      header?.forEach((th, idx) => {
        th.id = `assemblies_registry_table_th_${idx}`
      })
    }
  })
</script>

<style scoped lang="sass">
  @import "assembliesRegistryTableBox"
</style>
