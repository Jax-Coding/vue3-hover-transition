<script setup>
import { useEventListener, useRafFn } from "@vueuse/core";
import { interpolate, clamp, mapRange, gsap, Power2 } from "gsap/all";
import { ref, watch } from "vue";

const props = defineProps({
  items: {
    type: Array,
    requried: true,
  },
  activeIndex: {
    type: Number,
    requried: false,
    default: null,
  },
  offsets: {
    type: Object,
    required: true,
  },
});

const $box = ref(null);

const x = ref(0);
const y = ref(0);
const prevX = ref(0);
let rotation = 0;

const positionX = ref(0);
const positionY = ref(0);

useEventListener(window, "mousemove", (e) => {
  x.value = e.clientX;
  y.value = e.clientY;
});

useRafFn(() => {
  positionX.value = interpolate(positionX.value, x.value, 0.08);
  positionY.value = interpolate(positionY.value, y.value, 0.08);

  const distance = clamp(0, 100, Math.abs(prevX.value - x.value));
  rotation = interpolate(
    rotation,
    mapRange(0, 100, 0, prevX.value - x.value < 0 ? 45 : -45, distance),
    0.08
  );

  gsap.set($box.value, {
    rotation,
  });

  prevX.value = x.value;
});

const $images = ref([]);

watch(
  () => props.activeIndex,
  (current, prev) => {
    if (current !== null) {
      gsap.to($images.value[current], {
        opacity: 1,
        duration: 0.445,
        ease: Power2.easeInOut,
      });
    }

    if (prev !== null) {
      gsap.to($images.value[prev], {
        opacity: 0,
        duration: 0.445,
        ease: Power2.easeInOut,
      });
    }
  }
);

const $el = ref(null);

const animateIn = () => {
  gsap.killTweensOf($el.value);

  gsap.to($el.value, {
    clipPath: `inset(0% 0% 0% 0%)`,
    duration: 0.455,
    ease: Power2.easeInOut,
  });
};

const animateOut = () => {
  gsap.killTweensOf($el.value);

  gsap.to($el.value, {
    clipPath: `inset(0% 0% 100% 0%)`,
    duration: 0.455,
    ease: Power2.easeInOut,
  });
};

defineExpose({
  animateIn,
  animateOut,
});
</script>

<template>
  <div
    ref="$box"
    class="w-[30vw] h-[18vw] fixed top-0 left-0 z-10 -translate-x-1/2 -translate-y-1/2 pointer-events-none overflow-hidden"
    :style="{ top: `${positionY}px`, left: `${positionX}px` }"
  >
    <div
      ref="$el"
      class="w-full h-full relative"
      style="clip-path: inset(0% 0% 100% 0%)"
    >
      <div
        :ref="(el) => ($images[index] = el)"
        :key="item.title"
        v-for="(item, index) in items"
        class="w-full h-full absolute top-0 left-0 opacity-0"
        :style="{
          transform: `translateX(${
            12.5 * offsets[index]?.x || 0
          }%) translateY(${12.5 * offsets[index]?.y || 0}%) scale(1.25)`,
        }"
      >
        <img class="w-full h-full object-cover" :src="item.image" />
      </div>
    </div>
  </div>
</template>
