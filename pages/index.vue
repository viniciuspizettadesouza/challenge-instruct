<template>
  <div class="leads container">
    <Logo />
    <!-- <Logo dark-background /> -->
    <h1 class="leads__title">Leads</h1>
    <SelectFilterTable
      :name-options="nameOptions"
      :category-options="categoryOptions"
      :leads-json="leadsJson"
      @active-filter="updateFilter('filters', $event)"
      @leads-filtered="filterLeadsJson('leads-filtered', $event)"
    />
    {{ filter }}
    <IndexTable :leads-json="leadsJson" />
  </div>
</template>

<script>
import SelectFilterTable from '~/components/SelectFilterTable.vue'
import IndexTable from '~/components/IndexTable.vue'

export default {
  components: { SelectFilterTable, IndexTable },
  data() {
    return {
      leadsJson: [],
      nameOptions: [],
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
            this.getNameOptions()
            this.getCategoryOptions()
            this.updateOptions()
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
    },
    getNameOptions() {
      this.nameOptions = this.leadsJson.map((item) => item.name)
    },
    getCategoryOptions() {
      const options = this.filterCategoryOptions()
      this.categoryOptions = options.map((item) => item)
    },
    filterCategoryOptions() {
      let options = this.leadsJson.map((item) => item.company.bs)
      options = options.toString().split(/[ ,]+/).join(',')
      options = options.split(',')
      return options.filter((value, index) => options.indexOf(value) === index)
    },
    updateOptions() {
      const options = this.leadsJson
      options.map((item, index) => {
        options[index].company.bs = item.company.bs.split(' ')
        return null
      })
    },
    filterLeadsJson(key, event) {
      this.leadsJson = event
    },
    updateFilter(key, event) {
      this.filter = event
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
