<script setup>
import { computed } from "vue"

const { todos, categories } = defineProps({
  todos: Array,
  categories: Array,
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
  <div>
    <div class="text-center mt-8" v-show="!todos.length">
      <svg
        class="mx-auto h-12 w-12 text-gray-400"
        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        aria-hidden="true"
      >
        <path
          vector-effect="non-scaling-stroke"
          stroke-linecap="round"
          stroke-linejoin="round"
          stroke-width="2"
          d="M9 13h6m-3-3v6m-9 1V7a2 2 0 012-2h6l2 2h6a2 2 0 012 2v8a2 2 0 01-2 2H5a2 2 0 01-2-2z"
        />
      </svg>
      <h3 class="mt-2 text-sm font-semibold text-gray-900">No tasks</h3>
      <p class="mt-1 text-sm text-gray-500">Get started by creating a new task.</p>
    </div>
    <div class="sm:flex sm:items-center" v-show="todos.length">
      <div class="sm:flex-auto">
        <h1 class="text-base font-semibold text-gray-900">Tasks</h1>
        <p class="mt-2 text-sm text-gray-700">
          A list of all the tasks in your account including their name, category, date and status.
        </p>
      </div>
    </div>
    <div class="mt-8 flow-root" v-show="todos.length">
      <div class="-mx-4 -my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
        <div class="inline-block min-w-full py-2 align-middle sm:px-6 lg:px-8">
          <table class="min-w-full divide-y divide-gray-300">
            <thead>
              <tr>
                <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-0">Name</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Category</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Created at</th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">Status</th>
                <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-0">
                  <span class="sr-only">Edit</span>
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">
              <tr v-for="todo in sorted" :key="todo.createdAt">
                <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-0">
                  {{ todo.content }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ categories.find(c => c.id === todo.category).title }}</td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">{{ todo.createdAt }}</td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  {{ todo.isDone ? "Done" : "In progress" }}
                </td>
                <td class="relative whitespace-nowrap py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-0">
                  <a href="#" class="text-red-600 hover:text-red-900" @click="removeTodo(todo)">Delete</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
