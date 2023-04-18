<template>
  <div class="controls">
    <Select
      id="assembly_group_controls_build_types_group_select"
      v-model="operator"
      label="Группа видов строительства"
      placeholder="Выберите"
      :required="true"
    />
    <Select
      id="assembly_group_controls_system_select"
      v-model="status"
      label="Система"
      placeholder="Выберите"
      :required="true"
    />
    <Select
      id="assembly_group_controls_MEG_params_select"
      v-model="author"
      label="Параметры и атрибуты (МГЭ)"
      placeholder="Выберите"
    />
    <Button
      id="assembly_group_controls_save_button"
      class="controls-save"
      text="Сохранить"
      view="stroke"
      size="small"
    />
  </div>
</template>

<script setup lang="ts">
  const operator = ref<string>()
  const status = ref<string>()
  const author = ref<string>()
</script>

<style scoped lang="sass">
  @import "assemblyGroupControls"
</style>
