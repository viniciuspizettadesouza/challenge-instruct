<template>
  <span>
    <input v-model="nameFilter" />
    <select
      :disabled="categoryFilterDisabled"
      class="select-box"
      @change="changeCategoryOption($event)"
    >
      <option class="select-tip">Category Filter</option>
      <option v-for="option in categoryOptions" :key="option">
        {{ option }}
      </option>
    </select>
    <span hidden>{{ nameFilterComputed }}</span>
  </span>
</template>

<script>
export default {
  name: 'SelectFilterTable',
  props: {
    categoryOptions: {
      type: Array,
      default: null,
    },
    leadsFiltered: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {
      nameFilter: [],
      categoryFilter: [],
      categoryFilterDisabled: false,
    }
  },
  computed: {
    nameFilterComputed() {
      const nameSelected = this.nameFilter
      this.updateProp('name-filter', nameSelected)
      return nameSelected
    },
  },
  methods: {
    getCategoryFilter() {
      return this.categoryFilter
    },
    setCategoryFilter(item) {
      this.categoryFilter = item
    },
    changeCategoryOption(event) {
      const categorySelected = event.target.value
      if (!categorySelected) {
        return
      }
      const filter = this.getCategoryFilter()
      const selected = this.checkSelected(categorySelected, filter)
      if (filter.length > 1) {
        this.categoryFilterDisabled = true
      }
      if (!selected) {
        this.updateProp('category-filter', categorySelected)
        filter.push(categorySelected)
        this.setCategoryFilter(filter)
        return filter
      }
      this.updateProp('category-filter', categorySelected)
    },
    checkSelected(item, filter) {
      return filter.includes(item)
    },
    updateProp(key, item) {
      this.$emit(key, item)
      return item
    },
  },
}
</script>

<style lang="scss" scoped="true">
.select-box {
  width: 140px;
  height: 40px;
  outline: none;
  background: $border-color;
  border: none;
}
.select-tip {
  display: none;
}
</style>
