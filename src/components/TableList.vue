<script setup>
import { watch, ref } from "vue"

const { todos, categories } = defineProps({
  todos: Array,
  categories: Array,
})

const sorted = ref([...todos])
const sortKey = ref('createdAt')
const sortOrder = ref('desc')

const sortTodos = (key) => {
  if (sortKey.value === key) {
    sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortKey.value = key
    sortOrder.value = 'asc'
  }

  sorted.value = [...todos].sort((a, b) => {
    const modifier = sortOrder.value === 'asc' ? 1 : -1

    if (key === 'createdAt') {
      return (a[key] - b[key]) * modifier
    } else if (key === 'content' || key === 'category') {
      return a[key].localeCompare(b[key]) * modifier
    } else if (key === 'isDone') {
      return (a[key] === b[key] ? 0 : a[key] ? -1 : 1) * modifier
    } else {
      return 0
    }
  })
}

const removeTodo = (todo) => {
  const index = todos.indexOf(todo)

  if (index > -1) {
    todos.splice(index, 1)
  }
}

watch(() => todos, () => {
  sorted.value = [...todos].sort((a, b) => b.createdAt - a.createdAt)
}, { deep: true })
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
                <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pl-0">
                  <span class="sr-only">Mark as done</span>
                </th>
                <th scope="col" class="py-3.5 pl-4 pr-3 text-left text-sm font-semibold text-gray-900 sm:pl-0">
                  <a href="#" class="group inline-flex" @click.prevent="sortTodos('content')">
                    Name
                    <span class="ml-2 flex-none rounded text-gray-400 group-hover:visible group-focus:visible" :class="sortKey === 'content' ? 'bg-gray-100' : 'invisible'">
                      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true" data-slot="icon">
                        <path fill-rule="evenodd" d="M5.22 8.22a.75.75 0 0 1 1.06 0L10 11.94l3.72-3.72a.75.75 0 1 1 1.06 1.06l-4.25 4.25a.75.75 0 0 1-1.06 0L5.22 9.28a.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" v-show="sortKey === 'content' && sortOrder === 'asc'" />
                        <path fill-rule="evenodd" d="M9.47 6.47a.75.75 0 0 1 1.06 0l4.25 4.25a.75.75 0 1 1-1.06 1.06L10 8.06l-3.72 3.72a.75.75 0 0 1-1.06-1.06l4.25-4.25Z" clip-rule="evenodd" v-show="sortKey === 'content' && sortOrder === 'desc'" />
                      </svg>
                    </span>
                  </a>
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                  <a href="#" class="group inline-flex" @click.prevent="sortTodos('isDone')">
                    Status
                    <span class="ml-2 flex-none rounded text-gray-400 group-hover:visible group-focus:visible" :class="sortKey === 'isDone' ? 'bg-gray-100' : 'invisible'">
                      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true" data-slot="icon">
                        <path fill-rule="evenodd" d="M5.22 8.22a.75.75 0 0 1 1.06 0L10 11.94l3.72-3.72a.75.75 0 1 1 1.06 1.06l-4.25 4.25a.75.75 0 0 1-1.06 0L5.22 9.28a.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" v-show="sortKey === 'isDone' && sortOrder === 'asc'" />
                        <path fill-rule="evenodd" d="M9.47 6.47a.75.75 0 0 1 1.06 0l4.25 4.25a.75.75 0 1 1-1.06 1.06L10 8.06l-3.72 3.72a.75.75 0 0 1-1.06-1.06l4.25-4.25Z" clip-rule="evenodd" v-show="sortKey === 'isDone' && sortOrder === 'desc'" />
                      </svg>
                    </span>
                  </a>
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                  <a href="#" class="group inline-flex" @click.prevent="sortTodos('category')">
                    Category
                    <span class="ml-2 flex-none rounded text-gray-400 group-hover:visible group-focus:visible" :class="sortKey === 'category' ? 'bg-gray-100' : 'invisible'">
                      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true" data-slot="icon">
                        <path fill-rule="evenodd" d="M5.22 8.22a.75.75 0 0 1 1.06 0L10 11.94l3.72-3.72a.75.75 0 1 1 1.06 1.06l-4.25 4.25a.75.75 0 0 1-1.06 0L5.22 9.28a.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" v-show="sortKey === 'category' && sortOrder === 'asc'" />
                        <path fill-rule="evenodd" d="M9.47 6.47a.75.75 0 0 1 1.06 0l4.25 4.25a.75.75 0 1 1-1.06 1.06L10 8.06l-3.72 3.72a.75.75 0 0 1-1.06-1.06l4.25-4.25Z" clip-rule="evenodd" v-show="sortKey === 'category' && sortOrder === 'desc'" />
                      </svg>
                    </span>
                  </a>
                </th>
                <th scope="col" class="px-3 py-3.5 text-left text-sm font-semibold text-gray-900">
                  <a href="#" class="group inline-flex" @click.prevent="sortTodos('createdAt')">
                    Created at
                    <span class="ml-2 flex-none rounded text-gray-400 group-hover:visible group-focus:visible" :class="sortKey === 'createdAt' ? 'bg-gray-100' : 'invisible'">
                      <svg class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor" aria-hidden="true" data-slot="icon">
                        <path fill-rule="evenodd" d="M5.22 8.22a.75.75 0 0 1 1.06 0L10 11.94l3.72-3.72a.75.75 0 1 1 1.06 1.06l-4.25 4.25a.75.75 0 0 1-1.06 0L5.22 9.28a.75.75 0 0 1 0-1.06Z" clip-rule="evenodd" v-show="sortKey === 'createdAt' && sortOrder === 'asc'" />
                        <path fill-rule="evenodd" d="M9.47 6.47a.75.75 0 0 1 1.06 0l4.25 4.25a.75.75 0 1 1-1.06 1.06L10 8.06l-3.72 3.72a.75.75 0 0 1-1.06-1.06l4.25-4.25Z" clip-rule="evenodd" v-show="sortKey === 'createdAt' && sortOrder === 'desc'" />
                      </svg>
                    </span>
                  </a>
                </th>
                <th scope="col" class="relative py-3.5 pl-3 pr-4 sm:pr-0">
                  <span class="sr-only">Edit</span>
                </th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-200">
              <tr v-for="todo in sorted" :key="todo.createdAt">
                <td class="relative whitespace-nowrap py-4 pl-3 pr-4 text-left text-sm font-medium sm:pl-0">
                  <input
                    type="checkbox"
                    class="h-4 w-4 rounded border-gray-300 text-indigo-600 focus:ring-indigo-600" 
                    v-model="todo.isDone"
                  />
                </td>
                <td class="whitespace-nowrap py-4 pl-4 pr-3 text-sm font-medium text-gray-900 sm:pl-0" :class="`${todo.isDone && 'text-slate-400 line-through'}`">
                  {{ todo.content }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  <span class="inline-flex items-center gap-x-1.5 rounded-md px-2 py-1 text-xs font-medium text-gray-900 ring-1 ring-inset ring-gray-200" v-show="todo.isDone">
                    <svg class="h-1.5 w-1.5 fill-green-500" viewBox="0 0 6 6" aria-hidden="true">
                      <circle cx="3" cy="3" r="3" />
                    </svg>
                    Done
                  </span>
                  <span class="inline-flex items-center gap-x-1.5 rounded-md px-2 py-1 text-xs font-medium text-gray-900 ring-1 ring-inset ring-gray-200" v-show="!todo.isDone">
                    <svg class="h-1.5 w-1.5 fill-blue-500" viewBox="0 0 6 6" aria-hidden="true">
                      <circle cx="3" cy="3" r="3" />
                    </svg>
                    In progress
                  </span>
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  {{ categories.find(c => c.id === todo.category).title }}
                </td>
                <td class="whitespace-nowrap px-3 py-4 text-sm text-gray-500">
                  {{ new Date(todo.createdAt).toLocaleDateString() }} {{ new Date(todo.createdAt).toLocaleTimeString() }}
                </td>
                <td class="relative whitespace-nowrap py-4 pl-3 pr-4 text-right text-sm font-medium sm:pr-0">
                  <a href="#" class="text-red-600 hover:text-red-900" @click.prevent="removeTodo(todo)">Delete</a>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
