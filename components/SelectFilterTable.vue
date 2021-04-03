<template>
  <span>
    <select
      :disabled="nameFilterDisabled"
      class="selectBox disabled"
      @change="changeNameOption($event)"
    >
      <option class="select-tip">Name Filter</option>
      <option v-for="name in nameOptions" :key="name">
        {{ name }}
      </option>
    </select>
    <select
      :disabled="categoryFilterDisabled"
      class="selectBox"
      @change="changeCategoryOption($event)"
    >
      <option class="select-tip">Category Filter</option>
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
      nameFilter: [],
      categoryFilter: [],
      nameFilterDisabled: false,
      categoryFilterDisabled: false,
    }
  },
  methods: {
    changeNameOption(event) {
      const nameSelected = event.target.value
      if (!nameSelected) {
        return
      }
      let filter = this.nameFilter
      const selected = this.checkSelected(nameSelected, filter)
      if (selected !== true) {
        filter = this.addNameFilter(nameSelected)
      }
      this.filterLeadsByName(filter)
      this.nameFilterDisabled = true
    },
    changeCategoryOption(event) {
      const categorySelected = event.target.value
      if (!categorySelected) {
        return
      }
      let filter = this.categoryFilter
      const selected = this.checkSelected(categorySelected, filter)
      if (selected !== true) {
        filter = this.addCategoryFilter(categorySelected)
      }
      this.filterLeadsByCompany(filter)
      console.log(filter.length)
      if (filter.length > 2) {
        this.categoryFilterDisabled = true
      }
    },
    checkSelected(item, filter) {
      return filter.includes(item)
    },
    addNameFilter(item) {
      this.nameFilter.push(item)
      this.$emit('name-filter', this.nameFilter)
      return this.nameFilter
    },
    addCategoryFilter(item) {
      this.categoryFilter.push(item)
      this.$emit('company-filter', this.categoryFilter)
      return this.categoryFilter
    },
    filterLeadsByName(selected) {
      let leads = this.leadsJson
      leads = leads.filter((item) => {
        const name = selected.includes(item.name)
        if (name) {
          return item
        }
        return null
      })
      this.$emit('leads-filtered', leads)
    },
    filterLeadsByCompany(selected) {
      let leads = this.leadsJson
      leads = leads.filter((item) => {
        const company = item.company.bs.includes(selected.toString())
        if (company) {
          return item
        }
        return null
      })
      this.$emit('leads-filtered', leads)
    },
  },
}
</script>

<style lang="scss" scoped="true">
.selectBox {
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
