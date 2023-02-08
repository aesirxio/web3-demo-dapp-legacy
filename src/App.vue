<script setup>
import HeaderItem from "./components/HeaderItem.vue";
</script>

<template>
  <div class="flex flex-col h-full min-h-screen">
    <header class="container mx-auto flex-grow-0 py-3">
      <HeaderItem />
    </header>
    <main class="container mx-auto flex-grow-0">
      <div class="grid grid-cols-1 gap-0 w-full"><RouterView /></div>
    </main>
  </div>
</template>
