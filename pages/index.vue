<template>
  <div class="leads container">
    <Logo />
    <!-- <Logo dark-background /> -->
    <h1 class="leads__title">Leads</h1>
    <template>
      <span>
        <select v-model="nameSelected" @change="changeNameOption">
          <option v-for="name in nameOptions" :key="name">
            {{ name }}
          </option>
        </select>
      </span>
    </template>
    <template>
      <span>
        <select v-model="categorySelected" @change="changeCategoryOption">
          <option v-for="option in categoryOptions" :key="option">
            {{ option }}
          </option>
        </select>
      </span>
    </template>
    <IndexTable :leads-table="leadsTable" />
  </div>
</template>

<script>
import IndexTable from '~/components/IndexTable.vue'

export default {
  components: { IndexTable },
  data() {
    return {
      leadsJson: [],
      leadsTable: [],
      nameSelected: null,
      nameOptions: [],
      categorySelected: null,
      categoryOptions: [],
      filter: [],
    }
  },
  created() {
    this.fetchLeadsData()
  },
  methods: {
    fetchLeadsData() {
      fetch('https://jsonplaceholder.typicode.com/users').then((response) =>
        response
          .json()
          .then((json) => {
            this.leadsJson = json
            this.leadsTable = this.leadsJson
            this.filterNameOptions()
            this.filterCategoryOptions()
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
    },
    filterNameOptions() {
      this.nameOptions = this.leadsJson.map((item) => item.name)
    },
    filterCategoryOptions() {
      const options = this.getCategoryOptions()
      this.categoryOptions = options.map((item) => item)
    },
    getCategoryOptions() {
      let options = this.leadsJson.map((item) => item.company.bs)
      options = options.toString().split(/[ ,]+/).join(',')
      options = options.split(',')
      return options.filter((value, index) => options.indexOf(value) === index)
    },
    filterLeadsTable(selected) {
      this.leadsTable = this.leadsJson.filter(
        (item) => item.company.bs.includes(selected) || item.name === selected
      )
    },
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
      return this.filter
    },
  },
}
</script>

<style lang="scss" scoped="true">
.container {
  padding-right: 25px;
  padding-left: 25px;
}

.leads {
  &__title {
    margin: 1.4rem 0;
    padding: 1.4rem 0;
    border-top: $border-color 1px solid;
  }
}
</style>
