<template>
  <div class="assemblies-registry-settings">
    <div class="assemblies-registry-settings__row">
      <Select
        id="assemblies_registry_settings_operator"
        v-model="operator"
        option-label-key="fio"
        :options="userList"
        label="Исполнитель"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_status"
        v-model="status"
        option-label-key="name"
        :options="buildStateList"
        label="Статус"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_autor"
        v-model="autor"
        option-label-key="fio"
        :options="userList"
        label="Автор"
        placeholder="Выберите"
        size="small"
      />
    </div>
    <div class="assemblies-registry-settings__title">По виду</div>
    <div class="assemblies-registry-settings__row row-five">
      <Select
        id="assemblies_registry_settings_group_of_construction_types"
        v-model="groupOfConstructionTypes"
        option-label-key="name"
        :options="buildObjectTypeList"
        label="Вид объекта"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_system"
        v-model="system"
        option-label-key="name"
        :options="buildSystemList"
        label="Система"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_subsystem"
        v-model="subsystem"
        option-label-key="name"
        :options="buildSubsystemList"
        label="Подсистема"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_family"
        v-model="family"
        option-label-key="name"
        :options="buildClassList"
        label="Семейство"
        placeholder="Выберите"
        size="small"
      />
      <Select
        id="assemblies_registry_settings_instance"
        v-model="instance"
        option-label-key="name"
        :options="buildBIMSystemList"
        label="Экземпляр"
        placeholder="Выберите"
        size="small"
      />
    </div>
    <div class="assemblies-registry-settings__title">По дате создания</div>
    <div class="assemblies-registry-settings__range-date">
      2023-01-01 12:00:00 - 2023-01-01 12:00:00
    </div>
    <Button
      id="assemblies_registry_settings_forming_button"
      text="Сформировать перечень"
      view="smoke"
      size="small"
      disabled
    />
  </div>
</template>

<script setup lang="ts">
  import { getPrivateUsersRequest, getReferenceRequest, IUser } from '@/api'
  import { REFERENCE } from '@/enums'

  const emits = defineEmits<{
    (e: 'filter', value: object): void
  }>()

  const operator = ref()
  const status = ref()
  const autor = ref()
  const groupOfConstructionTypes = ref()
  const system = ref()
  const subsystem = ref()
  const family = ref()
  const instance = ref()

  emits('filter', { operator, status })

  const userList = ref<IUser[]>([])
  const buildStateList = ref<any[]>([])
  const buildObjectTypeList = ref<any[]>([])
  const buildSystemList = ref<any[]>([])
  const buildSubsystemList = ref<any[]>([])
  const buildClassList = ref<any[]>([])
  const buildBIMSystemList = ref<any[]>([])

  const fetchUserList = () => {
    getPrivateUsersRequest().then((result) => {
      if (result?.data) {
        userList.value = result.data.map((user) => ({
          ...user,
          fio: `${user.user.last_name} ${user.user.first_name} ${user.user.middle_name}`,
        }))
      }
    })
  }
  const fetchBuildStates = () => {
    getReferenceRequest(REFERENCE.build_states_demo).then((result) => {
      if (result?.data?.results) {
        buildStateList.value = result.data.results
      }
    })
  }
  const fetchBuildObjectTypes = () => {
    getReferenceRequest(REFERENCE.build_object_type).then((result) => {
      if (result?.data?.results) {
        buildObjectTypeList.value = result.data.results
      }
    })
  }
  const fetchBuildSystems = () => {
    getReferenceRequest(REFERENCE.build_system).then((result) => {
      if (result?.data?.results) {
        buildSystemList.value = result.data.results
      }
    })
  }
  const fetchBuildSubsystems = () => {
    getReferenceRequest(REFERENCE.build_subsystem).then((result) => {
      if (result?.data?.results) {
        buildSubsystemList.value = result.data.results
      }
    })
  }
  const fetchBuildClasses = () => {
    getReferenceRequest(REFERENCE.build_class).then((result) => {
      if (result?.data?.results) {
        buildClassList.value = result.data.results
      }
    })
  }
  const fetchBuildBIMSystems = () => {
    getReferenceRequest(REFERENCE.build_bim_system).then((result) => {
      if (result?.data?.results) {
        buildBIMSystemList.value = result.data.results
      }
    })
  }

  onMounted(() => {
    fetchUserList()
    fetchBuildStates()
    fetchBuildObjectTypes()
    fetchBuildSystems()
    fetchBuildSubsystems()
    fetchBuildClasses()
    fetchBuildBIMSystems()
  })
</script>

<style scoped lang="sass">
  @import "assembliesRegistrySettings"
</style>
