<template>
  <div class="text-center mb-3">
    <button
      type="button"
      class="btn btn-primary me-2"
      @click="$emit('onRandomContacClicked')"
    >
      Add Random Contact
    </button>
    <button
      type="button"
      class="btn btn-primary me-2"
      @click="$emit('onSortByPopulatityClicked')"
    >
      <i :class="sortByPopularityClass"></i> Sort by popularity
    </button>
    <button
      type="button"
      class="btn btn-primary me-2"
      @click="$emit('onSortByNameClicked')"
    >
      <i :class="sortByNameClass"></i> Sort by name
    </button>
  </div>
</template>

<script>
export default {
  name: "ButtonBar",
  props: {
    sortByPopularityClass: {
      type: String,
      required: true,
    },
    sortByNameClass: {
      type: String,
      required: true,
    },
  },
};
</script>

<style scoped>
</style>