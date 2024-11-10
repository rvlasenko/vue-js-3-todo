<script setup>
import { ref } from "vue"

const props = defineProps({
  todos: {
    type: Object,
    required: true,
  },
  categories: Array,
})

const inputContent = ref("")
const inputCategory = ref("no-category")

const addTodo = () => {
  if (inputContent.value.trim() === "" || inputCategory.value === null) {
    return
  }

  props.todos.push({
    content: inputContent.value,
    category: inputCategory.value,
    createdAt: new Date().getTime(),
    isDone: false,
  })

  inputContent.value = ""
  inputCategory.value = "no-category"
};
</script>

<template>
  <form @submit.prevent="addTodo" class="space-y-6">
    <div>
      <label
        for="content"
        class="block text-sm/6 font-medium text-gray-900 after:content-['*'] after:ml-0.5 after:text-red-500"
      >
        Task name
      </label>
      <div class="mt-2">
        <input
          type="text"
          id="content"
          class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 text-sm/6"
          placeholder="e.g. Make a call"
          v-model="inputContent"
        />
      </div>
    </div>

    <fieldset>
      <legend class="text-sm/6 font-semibold text-gray-900">
        Pick a category
      </legend>
      <p class="mt-1 text-sm/6 text-gray-600">How do you classify this task?</p>
      <div class="mt-6 sm:space-x-6 sm:flex">
        <div v-for="category in categories" :key="category.id" class="flex items-center">
          <input
            :id="category.id"
            :value="category.id"
            name="category"
            type="radio"
            class="h-4 w-4 border-gray-300 text-indigo-600 focus:ring-indigo-600"
            v-model="inputCategory"
          />
          <label :for="category.id" class="ml-3 block text-sm/6 font-medium text-gray-900">
            {{ category.title }}
          </label>
        </div>
      </div>
    </fieldset>

    <button
      type="submit"
      class="rounded-md bg-indigo-600 px-5 py-1.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
    >
      Create
    </button>
  </form>
</template>
