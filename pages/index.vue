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
    <IndexTable :leads-options="leadsOptions" />
  </div>
</template>

<script>
import IndexTable from '~/components/IndexTable.vue'

export default {
  components: { IndexTable },
  data() {
    return {
      leads: null,
      leadsOptions: null,
      nameSelected: null,
      nameOptions: null,
      categorySelected: null,
      categoryOptions: null,
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
            this.leads = json
            this.leadsOptions = this.leads
            this.filterNameOptions()
            this.filterCategoryOptions()
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
    },
    filterNameOptions() {
      this.nameOptions = this.leads.map((item) => item.name)
    },
    filterCategoryOptions() {
      const options = this.getCategoryOptions()
      this.categoryOptions = options.map((item) => item)
    },
    getCategoryOptions() {
      let options = this.leads.map((item) => item.company.bs)
      options = options.toString().split(/[ ,]+/).join(',')
      options = options.split(',')
      return options.filter((value, index) => options.indexOf(value) === index)
    },
    selectNameFiltered(name) {
      this.leadsOptions = this.leads.filter((item) => item.name === name)
    },
    selectCategoryFiltered(category) {
      this.leadsOptions = this.leads.filter((item) =>
        item.company.bs.includes(category)
      )
    },
    changeNameOption() {
      if (this.nameSelected != null) {
        this.selectNameFiltered(this.nameSelected)
      }
    },
    changeCategoryOption() {
      if (this.categorySelected != null) {
        this.selectCategoryFiltered(this.categorySelected)
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
