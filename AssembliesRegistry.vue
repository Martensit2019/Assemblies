<template>
  <div class="assemblies-registry">
    <p class="assemblies-registry__title">
      Укажите необходимые параметры для формирования перечня ПСМ
    </p>
    <div class="layout">
      <GridLayout
        v-model:layout="windowSettings"
        :col-num="12"
        :row-height="10"
        :is-draggable="true"
        :is-resizable="false"
        :is-mirrored="false"
        :vertical-compact="true"
        :margin="[10, 10]"
        :use-css-transforms="true"
      >
        <grid-item
          v-for="item in windowSettings"
          :key="item.i"
          :x="item.x"
          :y="item.y"
          :w="item.w"
          :h="item.h"
          :min-w="2"
          :min-h="2"
          :i="item.i"
        >
          <div v-if="item.i === '0'">
            <AssembliesRegistryWindow title="По общим параметрам">
              <AssembliesRegistrySettings @filter="filter" />                                               <--------------
            </AssembliesRegistryWindow>
          </div>
          <div v-if="item.i === '1'">
            <AssembliesRegistryWindow title="Изображение ТИМ элемента ПСМ">
              <AssembliesRegistrySlider />
            </AssembliesRegistryWindow>
          </div>
          <div v-if="item.i === '2'">
            <AssembliesRegistryWindow title="Реестр ПСМ">
              <AssembliesRegistryTableBox />
            </AssembliesRegistryWindow>
          </div>
          <div v-if="item.i === '3'">
            <AssembliesRegistryWindow title="Карточка ПСМ">
              <AssembliesRegistryCard />
            </AssembliesRegistryWindow>
          </div>
        </grid-item>
      </GridLayout>
    </div>
  </div>
</template>
<script setup lang="ts">
  import { GridLayout, GridItem } from 'vue3-grid-layout-next'
  import settings from './AssembliesRegistryWindowSettings'
  const windowSettings = settings
  const assembliesRegistryFilter = ref()                                          <-------------------
  const filter = (filterObject: object) => {                                      <-------------------
    assembliesRegistryFilter.value = filterObject                                 <-------------------
  }                                                                               <-------------------
  provide('assemblies-registry-filter', assembliesRegistryFilter)                 <-------------------
</script>
<style scoped lang="sass">
  @import "assembliesRegistry"
</style>
