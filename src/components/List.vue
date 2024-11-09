<script setup>
import { computed, defineProps } from 'vue'

const { todos } = defineProps({
    todos: Array,
})

const sorted = computed(() => {
    return todos.sort((a, b) => b.createdAt - a.createdAt)
})

const removeTodo = (todo) => {
    const index = todos.indexOf(todo)

    if (index > -1) {
        todos.splice(index, 1)
    }
}
</script>

<template>
    <div class="list">
        <div v-for="todo in sorted" :class="`todo-item ${todo.isDone && 'is-done'}`">
            <label>
                <input type="checkbox" v-model="todo.isDone">
                <span :class="`bubble ${todo.category}`"></span>
            </label>

            <div class="todo-content">
                <input type="text" v-model="todo.content">
            </div>

            <div class="todo-actions">
                <button class="delete" @click="removeTodo(todo)">Delete</button>
            </div>
        </div>
    </div>
</template>
