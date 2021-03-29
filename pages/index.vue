<template>
  <div class="leads container">
    <Logo />
    <!-- <Logo dark-background /> -->
    <h1 class="leads__title">Leads</h1>
    <table id="leads">
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Username</th>
        <th>Email</th>
        <th colspan="5">Address</th>
        <th>Phone</th>
        <th>Website</th>
        <th colspan="3">Company</th>
      </tr>
      <tr v-for="lead in leads" :key="lead.id">
        <td>{{ lead.id }}</td>
        <td>{{ lead.name }}</td>
        <td>{{ lead.username }}</td>
        <td>{{ lead.email }}</td>
        <td>{{ lead.address.street }}</td>
        <td>{{ lead.address.suite }}</td>
        <td>{{ lead.address.city }}</td>
        <td>{{ lead.address.zipcode }}</td>
        <td>{{ lead.address.geo }}</td>
        <td>{{ lead.phone }}</td>
        <td>{{ lead.website }}</td>
        <td>{{ lead.company.name }}</td>
        <td>{{ lead.company.catchPhrase }}</td>
        <td>{{ lead.company.bs }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      leads: null,
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
          })
          .catch((error) => {
            console.error('Failed retrieving information', error)
          })
      )
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

#leads {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: separate;
  border-spacing: 0 10px;
}

#leads td,
#leads th {
  padding: 12px;
  text-align: left;
}

#leads td:first-child {
  border-radius: 10px 0 0 10px;
  text-align: center;
}

#leads td:last-child {
  border-radius: 0 10px 10px 0;
}

#leads tr:nth-child(even) {
  background-color: $blue;
  color: $grey;
}

#leads tr:hover {
  background-color: $green;
  color: $dark-blue;
}

#leads tr {
  border-collapse: collapse;
  border-radius: 0 10px 10px 0;
}

#leads th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: $dark-blue;
  color: white;
}

#leads th:first-child {
  border-radius: 10px 0 0 10px;
}

#leads th:last-child {
  border-radius: 0 10px 10px 0;
}
</style>
