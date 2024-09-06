<script setup lang="ts">
import { type PropType } from "vue";

const props = defineProps({
    optionsList: {
        type: Array as PropType<string[]>,
        required: true,
        default: () => ["test1", "test2"]
    },
    listHeading: {
        type: String,
        required: false,
        default: () => "Please select one"
    }
})

const modelValue = defineModel<string>({ required: true })
</script>

<template>
    <select v-model="modelValue"
        class="bg-white border border-[#D1D1D1] text-[#32363D] text-sm rounded-full px-4 py-2 font-medium min-w-48 font-roboto">
        <option disabled value="">{{ props.listHeading }}</option>
        <option v-for="(option, index) in props.optionsList" :key="index.toString()" :value="option">
            {{ option }}
        </option>
    </select>
</template>