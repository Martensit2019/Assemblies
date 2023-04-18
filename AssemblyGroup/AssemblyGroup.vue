<template>
  <div class="assembly-group">
    <div class="assembly-group__wrapper parent">
      <div
        class="area area-zero"
        draggable="true"
        @dragstart.self="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_controls_window"
            title="Вид сборки"
            extra-style="shadowed"
          >
            <AssemblyGroupControls />
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-one"
        draggable="true"
        @dragstart.stop="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_assemblies_window"
            title="Сборки"
            extra-style="shadowed"
          >
            <Input
              id="assembly_group_assemblies_search"
              placeholder="Поиск"
              type="custom-search"
              class="assemblies-search"
            />
            <Assemblies />
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-two"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_forming_window"
            title="Фомирование сборки"
            extra-style="shadowed"
          >
            <AssembliesForming />
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-three"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_image_window"
            title="Изображение элемента Сборки Воздуховод"
            class="assembly-group__image__outer-container"
            extra-style="shadowed"
          >
            <div class="assembly-group__image__inner-container">
              <div class="assembly-group__image__arrow-container">
                <IconSprite
                  name="arrow_right"
                  class="assembly-group__image__arrow inverted"
                />
              </div>
              <AssembliesImage />
              <div class="assembly-group__image__arrow-container">
                <IconSprite
                  name="arrow_right"
                  class="assembly-group__image__arrow"
                />
              </div>
            </div>
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-four"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_estimate_base_window"
            title="Сметно-нормативная база ССС"
            extra-style="shadowed"
          >
            <Input
              id="assembly_group_estimate_base_search"
              placeholder="Поиск"
              type="custom-search"
              class="assemblies-search"
            />
            <AssembliesEstimateBase />
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-five"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_directory_window"
            extra-style="shadowed"
          >
            <AssembliesDirectory />
          </AssembliesWindow>
        </div>
      </div>
      <div
        class="area area-six"
        draggable="true"
        @dragstart="dragStart($event)"
        @dragover.prevent="dragOver($event)"
        @dragend="dragEnd($event)"
      >
        <div>
          <AssembliesWindow
            id="assembliy_group_params_window"
            title="Параметры и атрибуты"
            extra-style="shadowed"
          >
            <AssembliesParams />
          </AssembliesWindow>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
  const bigDiv = ref<HTMLDivElement | null>(null)
  const bigWidth = ref<number | null>(null)
  const bigHeight = ref<number | null>(null)

  const setWithToDragStart = (currentArea: HTMLDivElement) => {
    console.log('currentArea', currentArea)
    if (
      !currentArea.classList.contains('area-one') &&
      !currentArea.classList.contains('area-four')
    )
      return
    const currentAreaWidth = currentArea.clientWidth
    const currentAreaHeight = currentArea.clientHeight
    bigDiv.value = currentArea.querySelector(
      '.assemblies-box__inner'
    ) as HTMLDivElement
    bigWidth.value = bigDiv.value.getBoundingClientRect().width
    if (bigDiv.value && bigDiv.value.clientWidth > currentAreaWidth)
      bigDiv.value.style.width = currentAreaWidth + 'px'
    bigHeight.value = bigDiv.value.getBoundingClientRect().height
    if (bigDiv.value && bigDiv.value.clientHeight < currentAreaHeight) return
  }

  const dragStart = (e: any) => {
    const currentArea = e.target as HTMLDivElement
    currentArea.classList.add(`selected`)
    // setWithToDragStart(currentArea)
  }
  const dragOver = (e: Event) => {
    const parent = document.querySelector('.parent')
    const activeEl = document.querySelector('.selected')
    const currentEl = e.target
      ? (e.target as HTMLDivElement).closest('.area')
      : null
    if (activeEl !== currentEl) {
      const nextEl =
        currentEl === activeEl?.nextElementSibling
          ? currentEl?.nextElementSibling
          : currentEl
      if (activeEl && nextEl) {
        parent?.insertBefore(activeEl, nextEl)
      }
    }
  }
  const dragEnd = (e: Event) => {
    if (e.target) (e.target as HTMLDivElement).classList.remove(`selected`)
    if (bigDiv.value) bigDiv.value.style.width = bigWidth.value + 'px'
  }
</script>
<style scoped lang="sass">
  @import "assemblyGroup"
</style>
