<script setup>
const props = defineProps(["joke"])
</script>

<template>
  <div v-if="props.joke">
    <strong>{{ props.joke.setup }}</strong><br />
    {{ props.joke.punchline }}
  </div>
</template>