<template>
  <div class="w-full grid grid-cols-3 md:grid-cols-4 gap-4">
    <BoxBlock
      v-for="item in items"
      :key="item.order"
      :image="item.image"
      :order="item.order"
      :name="item.name"
    />
  </div>
</template>

<script lang="ts">
export default {
  props: ["items"],
};
</script>
