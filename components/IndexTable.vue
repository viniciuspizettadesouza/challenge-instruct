<template>
  <table id="leads">
    <tr>
      <th>ID</th>
      <th>Name</th>
      <th>Username</th>
      <th>Email</th>
      <th>Address</th>
      <th>Zipcode</th>
      <th>Phone</th>
      <th>Website</th>
      <th>Company</th>
      <th>Company categories</th>
    </tr>
    <tr v-for="lead in leadsJson" :key="lead.id">
      <td>{{ lead.id }}</td>
      <td>{{ lead.name }}</td>
      <td>{{ lead.username }}</td>
      <td>{{ lead.email }}</td>
      <td>
        {{ lead.address.city }}, {{ lead.address.street }},
        {{ lead.address.suite }}
      </td>
      <td>{{ lead.address.zipcode }}</td>
      <td>{{ lead.phone }}</td>
      <td>{{ lead.website }}</td>
      <td>{{ lead.company.name }}</td>
      <td>{{ lead.company.bs.join(', ') }}</td>
    </tr>
  </table>
</template>

<script>
export default {
  name: 'IndexTable',
  props: {
    leadsJson: {
      type: Array,
      default: null,
    },
  },
  data() {
    return {}
  },
}
</script>

<style lang="scss" scoped="true">
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

#leads tr:hover {
  background-color: $secondary;
  color: $default;
}

#leads tr {
  background-color: $border-color;
  color: $default;
  border-collapse: collapse;
  border-radius: 0 10px 10px 0;
}

#leads th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: $default;
  color: white;
}

#leads th:first-child {
  border-radius: 10px 0 0 10px;
}

#leads th:last-child {
  border-radius: 0 10px 10px 0;
}
</style>
