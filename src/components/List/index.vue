<script setup>
import { ref } from "vue";
import ListItem from "./ListItem.vue";
import ListBox from "./ListBox.vue";

defineProps({
  items: {
    type: Array,
    requried: true,
  },
});

const activeIndex = ref(null);
const box = ref(null);

const offsets = ref({});

const onMove = ({ x, y, index }) => {
  offsets.value[index] = { x, y };
};
</script>

<template>
  <ul @mouseenter="box.animateIn" @mouseleave="box.animateOut">
    <ListItem
      :key="item.title"
      v-for="(item, index) in items"
      :item="item"
      :index="index"
      @enter="activeIndex = $event"
      @move="onMove"
    />
  </ul>

  <ListBox
    ref="box"
    :items="items"
    :active-index="activeIndex"
    :offsets="offsets"
  />
</template>
