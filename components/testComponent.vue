<script setup lang="ts">
import { ref } from "vue";

const count = ref(0);

const increment = () => {
    count.value++;
}

const decrement = () => {
    if (count.value > 0) {
        count.value--;
    }
}
</script>

<template>
    <div class="component">
        <h1>Counter component</h1>
        <div class="sub-component">
            <button title="decrement the value" @click="decrement">-</button>
            <div class="counter">{{ count }}</div>
            <button title="increment the value" @click="increment">+</button>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.component {
    display: flex;
    flex-direction: column;
    background-color: white;
    font-size: 1rem;
    padding: 0em 4em 2em 4em;
    border: 1px solid lightgray;
    border-radius: 14px;
    text-align: center;
    gap: 2em;
    min-width: 500px;
    height: 100%;
}

.sub-component {
    display: flex;
    background-color: white;
    border: 1px solid lightgray;
    font-size: 1rem;
    padding: 2rem 4rem;
    border-radius: 0.5rem;
    gap: 2rem;
    justify-content: center;
    align-items: center;

    button {
        font-size: 1rem;
        background-color: blueviolet;
        border: none;
        padding: 0.2em 1em;
        color: white;
        font-weight: 600;
        border-radius: 5px;
        cursor: pointer;
    }

    button:hover {
        transition: 0.5s;
        box-shadow: 5px 5px 2px gray;
        padding: 0.21em 1.02em;
    }

    .counter {
        border: 1px solid lightgray;
        padding: 0.5em 1em;
        border-radius: 5px;

    }
}
</style>