<template>
  <input
    class="w-full md:w-auto border-gray-400 py-3 px-5 rounded-full shadow outline-none"
    type="text"
    placeholder="Search pokemon here..."
    @input="handleInput"
  />
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "SearchBar",
  methods: {
    handleInput(e: Event) {
      let text = (e.target as HTMLInputElement).value;
      this.$emit("on-type", text);
    },
  },
});
</script>
