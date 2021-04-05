<template>
  <div class="leads container">
    <Logo />
    <!-- <Logo dark-background /> -->
    <h1 class="leads__title">Leads</h1>
    <section class="test-container flex flex-wrap">
      <IndexFilterTable
        :category-options="categoryOptions"
        :leads-json="leadsFiltered"
        :category-filter-disabled="categoryFilterDisabled"
        @name-filter="updateLeads('name-filter', $event)"
        @category-filter="updateLeads('category-filter', $event)"
      />
      <IndexCategorySelected :category-filter="categoryFilter" />
    </section>
    <IndexTable :leads-filtered="leadsFiltered" />
  </div>
</template>

<script>
import IndexFilterTable from '~/components/IndexFilterTable.vue'
import IndexCategorySelected from '~/components/IndexCategorySelected.vue'
import IndexTable from '~/components/IndexTable.vue'

export default {
  components: { IndexFilterTable, IndexTable, IndexCategorySelected },
  data() {
    return {
      leadsJson: [],
      leadsFiltered: [],
      categoryOptions: [],
      categoryFilter: [],
      categoryFilterDisabled: false,
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
            this.setLeadsJson(json)
            this.setLeadsFiltered(json)
            this.getCategoryOptions()
            this.updateOptions()
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
    },
    getLeadsJson() {
      return this.leadsJson
    },
    setLeadsJson(item) {
      this.leadsJson = item
    },
    getLeadsFiltered() {
      return this.leadsFiltered
    },
    setLeadsFiltered(item) {
      this.leadsFiltered = item
    },
    getCategoryFilter() {
      return this.categoryFilter
    },
    setCategoryFilter(item) {
      this.categoryFilter.push(item)
    },
    setCategoryFilterDisabled(item) {
      this.categoryFilterDisabled = item
    },
    getCategoryOptions() {
      const options = this.filterCategoryOptions()
      const categoryOptions = options.map((item) => item)
      this.setCategoryOptions(categoryOptions)
    },
    setCategoryOptions(item) {
      this.categoryOptions = item
    },
    filterCategoryOptions() {
      const leadsJson = this.getLeadsJson()
      let options = leadsJson.map((item) => item.company.bs)
      options = options.toString().split(/[ ,]+/).join(',')
      options = options.split(',')
      return options.filter((value, index) => options.indexOf(value) === index)
    },
    updateOptions() {
      const leadsJson = this.getLeadsJson()
      leadsJson.map((item, index) => {
        leadsJson[index].company.bs = item.company.bs.split(' ')
        return null
      })
    },
    filterLeadsByName(selected) {
      if (selected.length < 1) {
        const leads = this.getLeadsJson()
        this.setLeadsFiltered(leads)
        return leads
      }
      let leads = this.getLeadsFiltered()
      selected = selected.toLowerCase()
      leads = leads.filter((item) => {
        const name = item.name.toLowerCase().includes(selected)
        if (name) {
          return item
        }
        return null
      })
      this.setLeadsFiltered(leads)
      return leads
    },
    filterLeadsByCategory(selected) {
      let leads = this.getLeadsFiltered()
      leads = leads.filter((item) => {
        const company = item.company.bs.includes(selected.toString())
        if (company) {
          return item
        }
        return null
      })
      const filter = this.getCategoryFilter()
      if (filter.includes(selected)) {
        this.setLeadsFiltered(leads)
        return null
      }
      this.checkFilterLength(filter)
      this.setCategoryFilter(selected)
      this.setLeadsFiltered(leads)
      return null
    },
    updateLeads(key, event) {
      if (key === 'name-filter') {
        this.filterLeadsByName(event)
        return
      }
      const categoryFilter = event.toString()
      this.filterLeadsByCategory(categoryFilter)
    },
    checkFilterLength(filter) {
      if (filter.length > 1) {
        this.setCategoryFilterDisabled(true)
        return null
      }
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
