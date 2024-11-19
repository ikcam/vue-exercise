<script setup>
import { computed, ref, watch } from 'vue'
import JokeComponent from './JokeComponent.vue'

const objectList = ref([])
const props = defineProps(['page', 'perPage', 'type'])
const currentPage = ref(1)
const totalPages = computed(() => Math.ceil(objectList.value.length / props.perPage))
const jokeList = computed(() => {
  const start = (currentPage.value - 1) * props.perPage
  const end = start + props.perPage
  return objectList.value.slice(start, end)
})

const fetchList = async () => {
  if (!props.type) return

  await fetch(`https://official-joke-api.appspot.com/jokes/${props.type}/ten`)
    .then(response => response.json())
    .then(data => {
      objectList.value = data
      currentPage.value = 1
    })
    .catch(err => console.error(err))
}

const setPage = (page) => {
  if (page < 1 || page > totalPages) return
  currentPage.value = page
}

watch(() => props.type, (newValue, oldValue) => {
  if (newValue === oldValue) return
  fetchList()
})
</script>

<template>
  <div v-if="props.type" class="space-y-5">
    <JokeComponent v-for="joke in jokeList" :key="joke.id" :joke="joke" />

    <div class="flex items-center justify-between">
      <button
        :disabled="currentPage <= 1"
        class="bg-gray-900 cursor-pointer px-4 py-2 text-gray-50 rounded-md shadow disabled:bg-gray-300 disabled:cursor-not-allowed"
        @click="setPage(currentPage - 1)"
      >
        Prev
      </button>

      Showing page {{ currentPage }} of {{ totalPages }}

      <button
        :disabled="currentPage >= totalPages"
        class="bg-gray-900 cursor-pointer px-4 py-2 text-gray-50 rounded-md shadow disabled:bg-gray-300 disabled:cursor-not-allowed"
        @click="setPage(currentPage + 1)"
      >
        Next
      </button>
    </div>
  </div>

  <div v-else class="py-20 text-center text-gray-500 text-sm">
    Select a category
  </div>
</template>