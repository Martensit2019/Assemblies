<template>
  <div class="assemblies-content">
    <div class="assemblies-content__inner">
      <ul class="tree-box area-assemblies-box">
        <li
          v-for="(assembly, idx) in assemblies"
          :key="assembly.id"
          :class="['tree-box__item', { indent: idx > 1 }]"
          :style="{ marginLeft: idx > 1 ? idx * 10 + 'px' : 0 }"
        >
          <div class="icon tree">
            <IconSprite name="minus" :width="20" :height="20" />
          </div>
          <div class="icon yellow">
            <IconSprite :name="assembly.img" :width="20.5" :height="20" />
          </div>
          <span>{{ assembly.title }}</span>
          {{ assembly.descr }}
        </li>
      </ul>
    </div>
  </div>
  <!-- <div class="assemblies-box">
    <div class="assemblies-box__wrapper">
      <div class="assemblies-box__inner">
        <div
          class="assemblies-box-item area-assemblies-box"
          draggable="true"
          @dragstart.stop="dragStart($event)"
          @dragend="dragEnd($event)"
          @dragover.prevent="dragOver($event)"
        >
          <div class="assemblies-box-item__top">
            <span>Сборка наименование</span>
            Устройство уплотняемых самоходными катками подстилающих слоев
          </div>
          <div class="assemblies-box-item__bottom">
            Дата создания <span>10.01.2023</span> Автор <span>Петров А.В</span>
          </div>
        </div>
        <div
          class="assemblies-box-item area-assemblies-box"
          draggable="true"
          @dragstart.stop="dragStart($event)"
          @dragend="dragEnd($event)"
          @dragover.prevent="dragOver($event)"
        >
          <div class="assemblies-box-item__top">
            <span>Экземпляр сборки </span>ACKOH
          </div>
          <div class="assemblies-box-item__bottom">
            Дата создания <span>10.01.2023</span> Автор <span>Петров А.В</span>
          </div>
        </div>
        <div class="tree-box__item pl">
          <IconSprite class="icon" name="plus" />
          <IconSprite class="icon" name="folder" />

          <span>??? . ??? .</span>
          Конструктивные решения
        </div>
      </div>
    </div>
  </div> -->
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
      img: 'fileGray',
      title: '11-3.',
      descr: 'Устройство уплотняемых самоходными катками подстилающих слоев',
    },
  ]
  const dragStart = (e: Event) => {
    if (e.target)
      (e.target as HTMLElement)
        .closest('.area-assemblies-box')
        ?.classList.add(`selected-assemblies-box`)
  }
  const dragOver = (e: Event) => {
    const parent = document.querySelector('.assemblies-box__inner')
    const activeEl = document.querySelector('.selected-assemblies-box')
    const currentEl = e.target
      ? (e.target as HTMLElement).closest('.area-assemblies-box')
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
    if (e.target)
      (e.target as HTMLElement).classList.remove(`selected-assemblies-box`)
  }
</script>

<style scoped lang="sass">
  @import "assemblies"
</style>
