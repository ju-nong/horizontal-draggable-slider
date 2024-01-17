<template>
    <div class="container" :style="`--size: ${arr.length};`">
        <div v-for="item in arr" :key="item">
            {{ item }}
        </div>
    </div>
</template>

<script setup lang="ts">
const arr = [1, 2, 3, 4, 5];
</script>

<style scoped>
.container {
    text-align: left;
    width: 100%;
    height: 250px;
    overflow-x: scroll;
    display: flex;
    box-sizing: border-box;
    border: 1px solid #000;
    scroll-snap-type: x mandatory;

    > div {
        flex: 0 0 100%;
        width: 100%;
        background-color: #663399;
        color: #fff;
        font-size: 30px;
        display: flex;
        align-items: center;
        justify-content: center;
        scroll-snap-align: start;

        text-align: center;
        font-size: 22px;
        font-weight: bold;

        &:nth-child(2n) {
            background-color: #fff;
            color: #663399;
        }
    }
}
</style>
