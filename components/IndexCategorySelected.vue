<template>
  <div class="flex flex-wrap">
    <div v-for="item in categoryFilter" :key="item" class="item-container">
      <p class="item">{{ item }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexCategorySelected',
  props: {
    categoryFilter: {
      type: Array,
      default: null,
    },
  },
}
</script>

<style lang="scss" scoped="true">
.item-container {
  background-color: $border-color;
  padding: 12px 20px;
  width: 100px;
  margin: 8px 0 8px 20px;
  font-size: 16px;
  border-radius: 4px;
}

.item {
  text-align: center;
}
</style>
