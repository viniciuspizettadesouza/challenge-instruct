<template>
  <div class="leads container">
    <Logo />
    <!-- <Logo dark-background /> -->
    <h1 class="leads__title">Leads</h1>
    <template>
      <span>
        <select v-model="nameSelected" @change="changeNameOption">
          <option v-for="(option, index) in nameOptions" :key="index">
            {{ option.name }}
          </option>
        </select>
      </span>
    </template>
    <IndexTable :leadsOptions="leadsOptions" />
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
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
    },
    filterNameOptions() {
      this.nameOptions = this.leads.map((item) => {
        return {
          name: item.name,
        }
      })
    },
    selectNameFiltered(name) {
      this.leadsOptions = this.leads.filter((item) => item.name === name)
    },
    changeNameOption() {
      if (this.nameSelected != null) {
        this.selectNameFiltered(this.nameSelected)
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
