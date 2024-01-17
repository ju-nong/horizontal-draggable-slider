<template>
    <!-- :style="`--size: ${list.length};`" -->
    <div class="slider" ref="$slider">
        <div class="slider-container">
            <slot />
        </div>

        <slot name="button"></slot>
    </div>
</template>

<script setup lang="ts">
import { toRefs, defineExpose, ref, computed } from "vue";

const props = defineProps({
    list: {
        default: [],
        type: Array,
        required: true,
    },
    button: {
        default: false,
        type: Boolean,
    },
});

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

function onPrev() {
    if (!$container.value) {
        return;
    }

    // 왼쪽 끝일 때
    if ($container.value.scrollLeft === 0) {
        return;
    }

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

    $container.value?.scrollBy({ left: itemWidth.value, behavior: "smooth" });
}

const { list } = props;

defineExpose({ onPrev, onNext });
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
        overflow-x: scroll;
        display: flex;
        scroll-snap-type: x mandatory;
    }
}
</style>
