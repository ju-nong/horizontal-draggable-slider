<template>
    <div>
        <h1>horizontal-draggable-slider</h1>
        <Sllider ref="$slider">
            <div v-for="item in arr" :key="item" class="item">
                {{ item }}
            </div>

            <template #button>
                <button class="controller prev" @click="handlePrev">
                    PREV
                </button>
                <button class="controller next" @click="handleNext">
                    NEXT
                </button>
            </template>

            <template #pagination>
                <div class="pagination">
                    <button
                        v-for="(_, index) in arr"
                        :key="index"
                        :class="[{ active: $slider?.nowIndex === index }]"
                        @click="handleMove(index)"
                    ></button>
                </div>
            </template>
        </Sllider>
    </div>
</template>

<script setup lang="ts">
const arr = [1, 2, 3, 4, 5];

import { ref } from "vue";
import Sllider from "./components/Sllider.vue";

const $slider = ref();

function handlePrev() {
    $slider.value.onPrev();
}

function handleNext() {
    $slider.value.onNext();
}

function handleMove(index: number) {
    $slider.value.onMove(index);
}
</script>

<style scoped>
h1 {
    padding: 8px 16px;
}

.item {
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

.controller {
    position: absolute;
    width: 50px;
    line-height: 50px;
    border-radius: 1rem;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;

    &.prev {
        left: 5px;
    }

    &.next {
        right: 5px;
    }
}

.pagination {
    position: absolute;
    left: 50%;
    bottom: 10px;

    transform: translateX(-50%);

    display: flex;
    align-items: center;
    justify-content: center;
    column-gap: 10px;

    > button {
        width: 8px;
        height: 8px;
        background-color: #fff;
        cursor: pointer;
        border: 1px solid #ccc;
        border-radius: 100%;

        &.active {
            background-color: #663399;
        }
    }
}
</style>
