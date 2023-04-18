<template>
  <div class="assemblies-content">
    <div class="assemblies-content__inner">
        <ul class="tree-box area-assemblies-box">
          <li
            v-for="(assembly, idx) in assemblies"
            :key="assembly.id"
            :class="['tree-box__item', { indent: idx > 1 }]"
          >
            <div class="icon">
              <IconSprite name="tree_collapse" :width="20" :height="20" />
            </div>
            <div class="icon">
              <IconSprite :name="assembly.img" :width="20" :height="20" />
            </div>
            <span>{{ assembly.title }}</span>
            {{ assembly.descr }}
          </li>
        </ul>
      </div>
    </div>
</template>

<script setup lang="ts">
  const assemblies = [
    {
      id: 1,
      img: 'folder',
      title: '001',
      descr: 'Административно-деловые обьекты',
    },

    { id: 2, img: 'folder', title: '010', descr: 'Многоквартирный дом' },
    {
      id: 3,
      img: 'folder',
      title: '010.001',
      descr: 'Многоэтажный многоквартирный дом',
    },

    {
      id: 4,
      img: 'folder',
      title: '010.001.001',
      descr: 'Многоэтажный многоквартирный дом',
    },
    {
      id: 5,
      img: 'folder',
      title: '??? . ??? .',
      descr: 'Архитектурные решения',
    },
    {
      id: 6,
      img: 'folder',
      title: '??? . ??? .',
      descr: 'Полы, потолки',
    },
    {
      id: 7,
      img: 'folder',
      title: '2001.3.',
      descr: 'Сборники норм и расценок на строительные работы',
    },
    {
      id: 8,
      img: 'folder',
      title: '2001.3. - 11',
      descr: 'Сборник 11. Полы',
    },
    {
      id: 9,
      img: 'file',
      title: '11-3.',
      descr: 'Устройство уплотняемых самоходными катками подстилающих слоев',
    },
  ]
</script>

<style scoped lang="sass">
  @import "assembliesEstimateBase"
</style>
