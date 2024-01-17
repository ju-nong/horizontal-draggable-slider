<template>
    <!-- :style="`--size: ${list.length};`" -->
    <div class="slider" ref="$slider">
        <div class="slider-content">
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
const $content = computed<HTMLDivElement | null>(() => {
    if ($slider.value) {
        const $content = $slider.value.querySelector(
            ".slider-content",
        ) as HTMLDivElement;

        return $content || null;
    }

    return null;
});
const contentLength = computed(() =>
    $content.value ? $content.value.children.length : 0,
);
const itemWidth = computed(() =>
    $content.value ? $content.value.scrollWidth / contentLength.value : 0,
);

function onPrev() {
    if (!$content.value) {
        return;
    }

    // 왼쪽 끝일 때
    if ($content.value.scrollLeft === 0) {
        return;
    }

    $content.value?.scrollBy({ left: -itemWidth.value, behavior: "smooth" });
}

function onNext() {
    if (!$content.value) {
        return;
    }

    // 오른쪽 끝일 때
    const { scrollLeft, scrollWidth } = $content.value;
    if (scrollLeft === scrollWidth) {
        return;
    }

    $content.value?.scrollBy({ left: itemWidth.value, behavior: "smooth" });
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

    .slider-content {
        width: 100%;
        height: 100%;
        overflow-x: scroll;
        display: flex;
        scroll-snap-type: x mandatory;
    }
}
</style>
