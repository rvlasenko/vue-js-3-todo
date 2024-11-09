<script setup>
import { ref, defineProps } from 'vue'

const props = defineProps({
    todos: {
        type: Object,
        required: true,
    },
});

const inputContent = ref('')
const inputCategory = ref(null)

const addTodo = () => {
    if (inputContent.value.trim() === '' || inputCategory.value === null) {
        return
    }

    props.todos.push({
        content: inputContent.value,
        category: inputCategory.value,
        createdAt: new Date().getTime(),
        isDone: false,
    })

    inputContent.value = ''
    inputCategory.value = null
}
</script>

<template>
    <form @submit.prevent="addTodo">
        <h4>What's on your todo list?</h4>
        <input type="text" placeholder="e.g. Make a call" v-model="inputContent">
        <h4>Pick a category</h4>
        <div class="options">
            <label>
                <input type="radio" name="category" value="business" v-model="inputCategory">
                <span class="bubble business"></span>
                <div>Business</div>
            </label>
            <label>
                <input type="radio" name="category" value="personal" v-model="inputCategory">
                <span class="bubble personal"></span>
                <div>Personal</div>
            </label>
        </div>

        <input type="submit" value="Add todo">
    </form>
</template>
