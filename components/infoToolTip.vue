<script setup lang="ts">
import { ref, computed } from "vue";

const props = defineProps({
    message: {
        type: String,
        required: true,
        default: () => "test message"
    }
});

const showTooltip = ref(false);
const tooltipText = computed(() => props.message.replace(/\r\n/g, '\n').trim());
</script>

<template>
    <div class="relative inline-block" @mouseover="showTooltip = true" @mouseleave="showTooltip = false">
        <button type="button" class="text-[#FF9A9F]"><i class="fa-solid fa-circle-info"></i></button>
        <div v-show="showTooltip"
            class="absolute z-10 w-52 p-2 bg-gray-800 text-white text-xs rounded-lg shadow-md transform left-1/2 -translate-x-1/2 mt-2">
            <div class="word-wrap break-words">{{ tooltipText }}</div>
            <div class="absolute top-0 left-1/2 transform -translate-x-1/2 -mt-1 w-2 h-2 bg-gray-800 rotate-45"></div>
        </div>
    </div>
</template>