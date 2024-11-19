<script setup>
import { ref } from 'vue'

const props = defineProps(["currentType"])
const emit = defineEmits(["select"])
const jokeTypes = ref([])
const loading = ref(true)

fetch('https://official-joke-api.appspot.com/types')
  .then(response => response.json())
  .then(data => jokeTypes.value = data)
  .catch(err => console.error(err))
  .finally(() => loading.value = false)
</script>

<template>
  <div class="space-y-1">
    <button
      v-for="jokeType in jokeTypes"
      :key="jokeType"
      class="block bg-gray-700 font-semibold px-4 py-2 text-gray-50 text-sm uppercase rounded w-full hover:bg-gray-600"
      :class="{'!bg-gray-500': props.currentType === jokeType}"
      @click="emit('select', jokeType)"
    >
      {{ jokeType }}
    </button>
  </div>
</template>