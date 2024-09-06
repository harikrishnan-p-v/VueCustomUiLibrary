<script setup lang="ts">
import { computed, type PropType } from 'vue';
import UiToolTip from "./toolTip.vue"

const props = defineProps({
    userName: {
        type: String,
        required: true,
        default: "testUser"
    },
    displayUserName: {
        type: Boolean,
        required: false,
        default: false
    },
    backgroundColors: {
        type: Array as PropType<string[]>,
        required: false,
        default: () => ["#0078d7", "#2E4053", "#333333", "#1A1D23", "#454545", "#635787", "#708090", "#343A40", "#353839", "#666666", "#4682B4"]
    },
    iconHeight: {
        type: String,
        required: false,
        default: "32px"
    },
    iconWidth: {
        type: String,
        required: false,
        default: "32px"
    },
    iconFontSize: {
        type: String,
        required: false,
        default: "12px"
    },
    userNameFontSize: {
        type: String,
        required: false,
        default: "16px"
    },
})

const initials = computed(() => {
    const splited = props.userName.split(' ');

    if (splited.length == 1) {
        return (props.userName.slice(0, 2)).toUpperCase();
    }
    else {
        return (splited[0].slice(0, 1) + splited[1].slice(0, 1)).toUpperCase();
    }
})

const getBackgroudColor = computed(() => {
    const randomIndex = Math.floor(Math.random() * props.backgroundColors.length);
    return `${props.backgroundColors[randomIndex]}`
});
</script>

<template>
    <UiToolTip :message="props.userName">
        <div class="user">
            <div class="user-icon">
                <span class="initials">{{ initials }}</span>
            </div>
            <p v-if="props.displayUserName" class="user-name">{{ props.userName }}</p>
        </div>
    </UiToolTip>
</template>

<style lang="css" scoped>
.user {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.5em;
}

.user-icon {
    height: v-bind('$props.iconHeight');
    width: v-bind('$props.iconWidth');
    border-radius: 50%;
    display: flex;
    justify-content: center;
    align-items: center;
    color: #ffffff;
    font-weight: bold;
    font-size: v-bind('$props.iconFontSize');
    background-color: v-bind('getBackgroudColor');
}

.user-name {
    font-size: v-bind('$props.userNameFontSize');
}
</style>