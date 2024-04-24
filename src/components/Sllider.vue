<template>
    <div class="slider" ref="$slider" :key="test">
        <div class="slider-container">
            <slot />
        </div>

        <slot name="button"></slot>
        <slot name="pagination"></slot>
    </div>
</template>

<script setup lang="ts">
import { defineExpose, ref, computed, nextTick } from "vue";

const test = ref(0);

const $slider = ref<HTMLDivElement | null>(null);
const $container = computed<HTMLDivElement | null>(() => {
    if ($slider.value) {
        const $container = $slider.value.querySelector(
            ".slider-container",
        ) as HTMLDivElement;

        return $container || null;
    }

    return null;
});
const itemLength = computed(() =>
    $container.value ? $container.value.children.length : 0,
);
const itemWidth = computed(() =>
    $container.value ? $container.value.scrollWidth / itemLength.value : 0,
);

const nowIndex = ref(0);

const $debouncing = ref<null | number>(null);
function setDebouncing() {
    $debouncing.value = setTimeout(() => {
        $debouncing.value = null;
    }, 500);
}

function onPrev() {
    if (!$container.value) {
        return;
    }

    // 왼쪽 끝일 때
    if ($container.value.scrollLeft === 0) {
        return;
    }

    // 이동 중일 때
    if ($debouncing.value !== null) {
        return;
    }

    nowIndex.value = nowIndex.value - 1;
    setDebouncing();

    $container.value?.scrollBy({ left: -itemWidth.value, behavior: "smooth" });
}

function onNext() {
    if (!$container.value) {
        return;
    }

    // 오른쪽 끝일 때
    const { scrollLeft, scrollWidth } = $container.value;
    if (scrollLeft === scrollWidth) {
        return;
    }

    // 이동 중일 때
    if ($debouncing.value !== null) {
        return;
    }

    nowIndex.value = nowIndex.value + 1;
    setDebouncing();

    $container.value?.scrollBy({ left: itemWidth.value, behavior: "smooth" });
}

function onMove(index: number) {
    if (!$container.value) {
        return;
    }

    // 이동 중일 때
    if ($debouncing.value !== null) {
        return;
    }

    nowIndex.value = index;
    setDebouncing();

    $container.value.scrollTo({
        left: itemWidth.value * index,
        behavior: "smooth",
    });
}

defineExpose({ onPrev, onNext, onMove, nowIndex });

// window.addEventListener("resize", async function () {
//     // test.value = test.value + 1;
//     console.log(itemWidth.value);
//     if ($slider.value) {
//         await nextTick();
//         $slider.value.scrollLeft = itemWidth.value * nowIndex.value;
//     }
// });
</script>

<style>
.slider {
    width: 100%;
    height: 250px;
    border: 1px solid #000;
    position: relative;

    .slider-container {
        width: 100%;
        height: 100%;
        display: flex;
        /* overflow-x: scroll;
        scroll-snap-type: x mandatory; */
        overflow-x: hidden;
    }
}
</style>
