<script setup>
import { ref, onMounted, watch } from "vue"
import Header from "./components/Header.vue"
import CreateForm from "./components/CreateForm.vue"
import TableList from "./components/TableList.vue"

const todos = ref([])

const categories = [
  { id: "no-category", title: "No category" },
  { id: "personal", title: "Personal" },
  { id: "business", title: "Business" },
  { id: "important", title: "Important" },
]

onMounted(() => {
  todos.value = JSON.parse(localStorage.getItem("todos")) || []
})

watch(todos, (newVal) => {
  localStorage.setItem("todos", JSON.stringify(newVal))
}, { deep: true })
</script>

<template>
  <div class="py-10">
    <Header />
    <main>
      <div class="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
        <CreateForm :todos="todos" :categories="categories" />
      </div>
      <div class="mx-auto max-w-7xl px-4 py-8 sm:px-6 lg:px-8">
        <TableList :todos="todos" :categories="categories" />
      </div>
    </main>
  </div>
</template>
