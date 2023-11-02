<template>
  <div class="w-full flex gap-2 justify-between items-center">
    <label class="relative inline-flex items-center cursor-pointer">
      <input type="checkbox" :checked="filter.value" class="sr-only peer" @change="onFilterChange(filter.key, $event.target.checked ? '1' : '0')">
      <div
        class="peer-focus:outline-none peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:transition-all"
        :class="getTheme('toggle', filter.value === null ? `${preStyle}_disabled` : preStyle)"
      />
    </label>
    <button
      :class="getTheme('reset_toggle_button', preStyle)"
      @click.prevent="onFilterChange(filter.key, null)"
    >
      <span class="sr-only">Remove search</span>
      <svg
        xmlns="http://www.w3.org/2000/svg"
        class="h-5 w-5"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
      >
        <path
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M6 18L18 6M6 6l12 12"
        />
      </svg>
    </button>
  </div>
</template>

<script setup>
import {inject} from "vue";

const props = defineProps({
  filter: {
    type: Object,
    required: true,
  },

  onFilterChange: {
    type: Function,
    required: true,
  },

  preStyle: {
    type: String,
    default: 'default',
    required: false,
  },
});

// Theme
const commonToggleClasses = "w-11 h-6 rounded-full after:bg-white after:border-white after:border after:rounded-full after:h-5 after:w-5"
const commonResetToggleButtonClasses = "rounded-md text-gray-400 hover:text-gray-500 focus:outline-none focus:ring-2 focus:ring-offset-2"

const fallbackTheme = {
  inertia_table: {
    table_filter: {
      toggle: {
        default: `${commonToggleClasses} peer-checked:bg-indigo-500 bg-red-500`,
        dootix: `${commonToggleClasses} peer-checked:bg-gradient-to-r peer-checked:from-cyan-500 peer-checked:to-blue-600 bg-red-500`,
        default_disabled: `${commonToggleClasses} bg-gray-200`,
        dootix_disabled: `${commonToggleClasses} bg-gray-200`,
      },
      reset_toggle_button: {
        default: `${commonResetToggleButtonClasses} focus:ring-indigo-500`,
        dootix: `${commonResetToggleButtonClasses} focus:ring-cyan-500`,
      },
    },
  },
}
const themeVariables = inject('themeVariables');
const getTheme = (type, name) => {
  if (
    "inertia_table" in themeVariables &&
    "table_filter" in themeVariables.inertia_table &&
    type in themeVariables.inertia_table.table_filter &&
    name in themeVariables.inertia_table.table_filter[type]
  ) {
    return themeVariables.inertia_table.table_filter[type][name];
  } else {
    return fallbackTheme.inertia_table.table_filter[type][name];
  }
}
</script>
