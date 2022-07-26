<script setup>
import { computed, ref } from "vue";
import { gsap, Power2, interpolate } from "gsap/all";

const emit = defineEmits(["enter", "move"]);

const props = defineProps({
  item: {
    type: Object,
    required: true,
  },
  index: {
    type: Number,
    required: true,
  },
});

const number = computed(() => `${props.index + 1}`.padStart(2, "0"));
const $el = ref(null);

const onEnter = () => {
  emit("enter", props.index);
  gsap.killTweensOf($el.value);

  gsap.to($el.value, {
    paddingLeft: 0,
    duration: 0.335,
    ease: Power2.easeInOut,
  });
};

const onLeave = () => {
  gsap.killTweensOf($el.value);

  gsap.to($el.value, {
    paddingLeft: "9rem",
    duration: 0.335,
    ease: Power2.easeInOut,
  });
};

const onMove = (e) => {
  const { x: left, y: top, width, height } = $el.value.getBoundingClientRect();

  const progressionY = (1 / height) * (e.clientY - top);
  const progressionX = (1 / width) * (e.clientX - left);

  const x = interpolate(-1, 1, progressionX);
  const y = interpolate(-1, 1, progressionY);

  emit("move", { x, y, index: props.index });
};
</script>

<template>
  <li class="border-b border-gray-400 cursor-pointer">
    <div
      ref="$el"
      @mouseenter="onEnter"
      @mouseleave="onLeave"
      @mousemove="onMove"
      class="pl-32 py-12 flex items-start space-x-8 relative z-20"
    >
      <span class="text-sm text-gray-500 translate-y-3">({{ number }})</span>
      <h3 class="text-8xl">{{ item.title }}</h3>
    </div>
  </li>
</template>
