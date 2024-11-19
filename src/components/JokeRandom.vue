<script setup>
import {ref} from 'vue'
import JokeComponent from './JokeComponent.vue'

const joke = ref(null)

fetch('https://official-joke-api.appspot.com/random_joke')
    .then(response => response.json())
    .then(data => joke.value = data)
    .catch(err => console.error(err))
</script>

<template>
  <JokeComponent :joke="joke" />
</template>