<template>
  <select
    name="per_page"
    :dusk="dusk"
    :value="value"
    :class="getTheme('select', preStyle)"
    @change="onChange($event.target.value)"
  >
    <option
      v-for="option in perPageOptions"
      :key="option"
      :value="option"
    >
      {{ option }} {{ translations.per_page }}
    </option>
  </select>
</template>

<script setup>
import {computed, inject} from "vue";
import uniq from "lodash-es/uniq";
import { getTranslations } from "../translations.js";

const translations = getTranslations();

const props = defineProps({
    dusk: {
        type: String,
        default: null,
        required: false,
    },

    value: {
        type: Number,
        default: 15,
        required: false,
    },

    options: {
        type: Array,
        default() {
            return [15, 30, 50, 100];
        },
        required: false,
    },

    onChange: {
        type: Function,
        required: true,
    },

    preStyle: {
        type: String,
        default: 'default',
        required: false,
    },
});

const perPageOptions = computed(() => {
    let options = [...props.options];

    options.push(parseInt(props.value));

    return uniq(options).sort((a, b) => a - b);
});

// Theme
const commonClasses = "block min-w-max shadow-sm text-sm border-gray-300 rounded-md"
const fallbackTheme = {
    inertia_table: {
        per_page_selector: {
            select: {
                default: `${commonClasses} focus:ring-indigo-500 focus:border-indigo-500`,
                dootix: `${commonClasses} focus:ring-cyan-500 focus:border-blue-500`,
            },
        },
    },
}
const themeVariables = inject('themeVariables');
const getTheme = (type, name) => {
    if (
        "inertia_table" in themeVariables &&
        "per_page_selector" in themeVariables.inertia_table &&
        type in themeVariables.inertia_table.per_page_selector &&
        name in themeVariables.inertia_table.per_page_selector[type]
    ) {
        return themeVariables.inertia_table.per_page_selector[type][name];
    } else {
        return fallbackTheme.inertia_table.per_page_selector[type][name];
    }
}
</script>

