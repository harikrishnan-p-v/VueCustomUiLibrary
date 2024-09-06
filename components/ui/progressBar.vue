<script setup lang="ts">
import { computed, type PropType } from 'vue';

interface progressbarTier {
    tierLimit: number,
    tierColor: string
}

const props = defineProps({
    progressbarValue: {
        type: Number,
        required: true,
        default: 50
    },
    progressbarTiers: {
        type: Array as PropType<progressbarTier[]>,
        required: false,
        default: () => [
            {
                tierLimit: 0,
                tierColor: '#D63030'
            },
            {
                tierLimit: 50,
                tierColor: '#3663AE'
            },
            {
                tierLimit: 75,
                tierColor: '#65BD4A'
            }
        ]
    }
});

const progressbarWidth = computed(() => {
    return props.progressbarValue <= 100 ? `${props.progressbarValue}%` : '100%';
});

const progressbarClass = computed(() => {
    if (props.progressbarValue >= props.progressbarTiers[2].tierLimit) {
        return "high";
    }
    else if (props.progressbarValue >= props.progressbarTiers[1].tierLimit) {
        return "medium";
    }
    else {
        return "low";
    }
});
</script>

<template>
    <div class="w-full flex gap-2 items-center">
        <div class="w-full bg-gray-200 rounded-full h-1.5 dark:bg-gray-700">
            <div :class="progressbarClass"></div>
        </div>
        <label class="text-[12px] text-[#34465F] font-medium">{{ props.progressbarValue +
            "%"
            }}</label>
    </div>
</template>

<style lang="css" scoped>
.low {
    width: v-bind('progressbarWidth');
    height: 0.375rem;
    border-radius: 9999px;
    background-color: v-bind('props.progressbarTiers[0].tierColor');
}

.medium {
    width: v-bind('progressbarWidth');
    height: 0.375rem;
    border-radius: 9999px;
    background-color: v-bind('props.progressbarTiers[1].tierColor');
}

.high {
    width: v-bind('progressbarWidth');
    height: 0.375rem;
    border-radius: 9999px;
    background-color: v-bind('props.progressbarTiers[2].tierColor');
}
</style>