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
</script>

<style scoped>
div {
    > h1 {
        padding: 8px 16px;
    }
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
</style>
