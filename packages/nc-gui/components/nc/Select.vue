<script lang="ts" setup>
const props = defineProps<{
  value?: string | string[]
  placeholder?: string
  mode?: 'multiple' | 'tags'
  dropdownClassName?: string
  showSearch?: boolean
  // filterOptions is a function
  filterOption?: (input: string, option: any) => boolean
  dropdownMatchSelectWidth?: boolean
  allowClear?: boolean
  loading?: boolean
}>()

const emits = defineEmits(['update:value', 'change'])

const placeholder = computed(() => props.placeholder)

const dropdownClassName = computed(() => {
  let className = 'nc-select-dropdown'
  if (props.dropdownClassName) {
    className += ` ${props.dropdownClassName}`
  }
  return className
})

const showSearch = computed(() => props.showSearch)

const filterOption = computed(() => props.filterOption)

const dropdownMatchSelectWidth = computed(() => props.dropdownMatchSelectWidth)

const loading = computed(() => props.loading)

const mode = computed(() => props.mode)

const vModel = useVModel(props, 'value', emits)

const onChange = (value: string) => {
  emits('change', value)
}
</script>

<template>
  <a-select
    v-model:value="vModel"
    :allow-clear="allowClear"
    :disabled="loading"
    :dropdown-class-name="dropdownClassName"
    :dropdown-match-select-width="dropdownMatchSelectWidth"
    :filter-option="filterOption"
    :loading="loading"
    :mode="mode"
    :placeholder="placeholder"
    :show-search="showSearch"
    class="nc-select"
    @change="onChange"
  >
    <template #suffixIcon>
      <GeneralLoader v-if="loading" />
      <GeneralIcon v-else class="text-gray-800 nc-select-expand-btn" icon="arrowDown" />
    </template>
    <slot />
  </a-select>
</template>

<style lang="scss">
.ant-select-item {
  @apply !xs:h-13 !min-h-[2.375rem] !p-2;
}
.ant-select-item-option-content {
  @apply !xs:mt-2.5;
}
.ant-select-item-option-state {
  @apply !xs:mt-1.75;
}
.ant-select-item-option {
  @apply !rounded-md;
}

.nc-select.ant-select {
  height: fit-content;
  .ant-select-selector {
    box-shadow: 0px 5px 3px -2px rgba(0, 0, 0, 0.02), 0px 3px 1px -2px rgba(0, 0, 0, 0.06);
    @apply border-1 border-gray-200 rounded-lg;
  }

  .ant-select-selection-item {
    @apply font-medium pr-3 rounded-md;
  }

  .ant-select-selection-placeholder {
    @apply text-gray-600;
  }
  .ant-select-selection-item-remove {
    @apply text-gray-800 !pb-1;
  }
}
.nc-select.ant-select-focused:not(.ant-select-disabled).ant-select:not(.ant-select-customize-input) .ant-select-selector {
  box-shadow: none;
  @apply border-brand-500;
}

.nc-select.ant-select.ant-select-disabled .nc-select-expand-btn {
  @apply text-gray-300;
}

.nc-select-dropdown {
  @apply !rounded-xl py-1.5;

  .rc-virtual-list-holder {
    overflow-y: scroll;
    overflow-x: hidden;
    font-weight: 500;

    .ant-select-item-option-content {
      font-weight: 500;
    }

    &::-webkit-scrollbar {
      width: 4px;
      height: 4px;
    }
    &::-webkit-scrollbar-track-piece {
      width: 0px;
    }
    &::-webkit-scrollbar {
      @apply bg-transparent;
    }
    &::-webkit-scrollbar-thumb {
      width: 4px;
      @apply bg-gray-300;
    }
    &::-webkit-scrollbar-thumb:hover {
      @apply bg-gray-400;
    }
  }
}
</style>
