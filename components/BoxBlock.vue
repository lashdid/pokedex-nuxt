<template>
  <div class="w-full shadow rounded p-3">
    <img class="mx-auto" :src="`${image}`" :alt="`${order}-${name}`" />
    <p class="text-center text-gray-400">#{{ order }}</p>
    <p class="text-center">{{ name }}</p>
  </div>
</template>

<script lang="ts">

export default {
  props: {
    image: String,
    order: Number,
    name: String,
  },
}
</script>
