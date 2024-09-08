<template>
    <div class="popup">
        <div class="loader"></div>
    </div>
</template>

<style lang="css" scoped>
.popup {
    background-color: rgba(0, 0, 0, 0.9);
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    justify-content: center;
    align-items: center;
    display: grid;
    z-index: 9999999;
}

.loader {
    width: 80px;
    aspect-ratio: 1;
    --_c: no-repeat radial-gradient(farthest-side, #f5f5f5 92%, #0000);
    background:
        var(--_c) top,
        var(--_c) left,
        var(--_c) right,
        var(--_c) bottom;
    background-size: 20px 20px;
    animation: l7 0.5s infinite;
}

@keyframes l7 {
    to {
        transform: rotate(.5turn)
    }
}
</style>