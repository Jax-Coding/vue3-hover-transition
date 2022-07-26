<script setup>
import List from "./components/List/index.vue";

const items = [
  {
    title: "Nike Inc.",
    image:
      "https://images.unsplash.com/photo-1461896836934-ffe607ba8211?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80",
  },
  {
    title: "Acne Studio",
    image:
      "https://images.unsplash.com/photo-1585914924626-15adac1e6402?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1471&q=80",
  },
  {
    title: "Chupa Chups",
    image:
      "https://images.unsplash.com/photo-1596793453564-31e6c88d67b1?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=687&q=80",
  },
  {
    title: "Asian Cusine",
    image:
      "https://images.unsplash.com/photo-1622643944007-450264a5f9a9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1444&q=80",
  },
];
</script>

<template>
  <div class="px-10 py-32">
    <List :items="items" />
  </div>
</template>
