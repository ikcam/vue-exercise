<script setup>
import { ref } from 'vue'
import JokeList from './components/JokeList.vue'
import JokeRandom from './components/JokeRandom.vue'
import JokeTypes from './components/JokeTypes.vue'

const currentType = ref(null)

const chooseType = (type) => {
  if (type === currentType.value) {
    currentType.value = null
    return
  }

  currentType.value = type
}
</script>

<template>
  <div>
    <header class="mb-5 px-4">
      <h1 class="font-semibold font-serif text-4xl text-blue-800">Jokes</h1>
    </header>
    <div class="container mx-auto lg:space-x-5 lg:flex">
      <aside class="mb-5 px-4 lg:mb-0 lg:px-0 lg:w-1/3">
        <JokeTypes :current-type="currentType" @select="chooseType" />
      </aside>
      <main class="px-4 w-full">
        <JokeList :type="currentType" :per-page="1" />
      </main>
    </div>
    <footer>
      <div class="container mx-auto pt-5">
        <JokeRandom class="text-right text-sm text-gray-500" />
      </div>
    </footer>
  </div>
</template>
