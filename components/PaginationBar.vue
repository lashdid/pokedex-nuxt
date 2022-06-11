<template>
  <div>
    <button class="mr-5" v-show="prevButton" @click="onClickPrev()">
      Prev
    </button>
    <button v-show="nextButton" @click="onClickNext()">Next</button>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "PaginationBar",
  props: {
    prevButton: String,
    nextButton: String,
  },
  methods: {
    onClickPrev() {
      this.$emit("click-prev");
    },
    onClickNext() {
      this.$emit("click-next");
    },
  },
});
</script>
