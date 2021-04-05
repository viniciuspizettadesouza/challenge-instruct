<template>
  <div>
    <input v-model="nameFilter" class="input" placeholder="Search name..." />
    <select
      :disabled="categoryFilterDisabled"
      class="input"
      @change="changeCategoryOption($event)"
    >
      <option class="select-option">Search Category</option>
      <option v-for="option in categoryOptions" :key="option">
        {{ option }}
      </option>
    </select>
    <span hidden>{{ nameFilterComputed }}</span>
  </div>
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
    categoryFilterDisabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      nameFilter: [],
      categoryFilter: [],
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
.select-option {
  display: none;
  font-size: 16px;
}

.input {
  width: 180px;
  padding: 12px 20px;
  margin: 8px 0;
  display: inline-block;
  border: none;
  border-radius: 4px;
  box-sizing: border-box;
  outline: none;
  color: $default;
  background-color: $border-color;
}

.input {
  font-size: 16px;
}
</style>
