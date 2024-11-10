<script setup>
import { ref, onMounted, watch } from "vue"

const name = ref("")

watch(name, (newVal) => {
  localStorage.setItem("name", newVal)
})

onMounted(() => {
  name.value = localStorage.getItem("name") || ""
})
</script>

<template>
  <header>
    <div class="mx-auto flex gap-2 max-w-7xl px-4 sm:px-6 lg:px-8">
      <h1 class="text-3xl font-bold tracking-tight text-gray-900">Hello,</h1>
      <input
        type="text"
        class="bg-transparent block border-0 focus:ring-0 font-bold p-0 text-3xl text-gray-900 placeholder-slate-500 w-full"
        placeholder="Jane Smith"
        v-model="name"
      />
    </div>
  </header>
</template>
