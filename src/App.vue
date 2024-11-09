<script setup>
import { ref, onMounted, watch } from 'vue'
import Greeting from './components/Greeting.vue'
import CreateForm from './components/CreateForm.vue'
import List from './components/List.vue'

const todos = ref([])

onMounted(() => {
    todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

watch(todos, (newVal) => {
    localStorage.setItem('todos', JSON.stringify(newVal))
}, { deep: true })
</script>

<template>
    <main class="app">
        <Greeting />

        <section class="create-todo">
            <h3>Create a todo</h3>
            <CreateForm :todos="todos" />
        </section>

        <section class="todo-list">
            <h3>Todo List</h3>
            <List :todos="todos" />
        </section>
    </main>
</template>
