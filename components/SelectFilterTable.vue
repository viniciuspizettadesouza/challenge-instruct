<template>
  <span>
    <select v-model="nameSelected" @change="changeNameOption">
      <option v-for="name in nameOptions" :key="name">
        {{ name }}
      </option>
    </select>
    <select v-model="categorySelected" @change="changeCategoryOption">
      <option v-for="option in categoryOptions" :key="option">
        {{ option }}
      </option>
    </select>
  </span>
</template>

<script>
export default {
  name: 'SelectFilterTable',
  props: {
    nameOptions: {
      type: Array,
      default: null,
    },
    categoryOptions: {
      type: Array,
      default: null,
    },
    leadsJson: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {
      nameSelected: null,
      categorySelected: null,
      filter: [],
    }
  },
  methods: {
    changeNameOption() {
      const nameSelected = this.nameSelected
      if (!nameSelected) {
        return
      }
      this.updateFilter(nameSelected)
    },
    changeCategoryOption() {
      const categorySelected = this.categorySelected
      if (!categorySelected) {
        return
      }
      this.updateFilter(categorySelected)
    },
    updateFilter(item) {
      let filter = this.filter
      const selected = this.checkSelected(item, filter)
      if (selected !== true) {
        filter = this.addFilter(item)
      }
      this.filterLeadsTable(filter)
    },
    checkSelected(item, filter) {
      return filter.includes(item)
    },
    addFilter(item) {
      this.filter.push(item)
      this.$emit('active-filter', this.filter)
      return this.filter
    },
    filterLeadsTable(selected) {
      const leads = this.leadsJson.filter(
        (item) =>
          item.company.bs.includes(selected) || item.name.includes(selected)
      )
      this.$emit('leads-filtered', leads)
    },
  },
}
</script>

<style scoped></style>
