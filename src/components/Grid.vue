<template>
  <div class="grid">

    <h1>{{ msg }}</h1>

    <div>
      <div class="filters">
        <div class="filter">
          <span class="labelName">Name: </span>
          <input class="name" type="text" v-model="searchTerm"/>
        </div>
        <div class="filter">
          <span class="labelName">Level: </span>
          <select class="level" v-model="selected" >
            <option value="">All</option>
            <option value="Standard">Standard</option>
            <option value="Silver">Silver</option>
            <option value="Gold">Gold</option>
          </select>
        </div>
      </div>


      <table>
        <thead>
          <tr>
            <template v-if="show.name">
              <th><span class="label">Customer Name</span>
                <img v-on:click="sort('name')" src="../assets/icon-sort.png" />
                <img v-on:click="hide('name')" src="../assets/icon-hide.png" />
              </th>
            </template>
            <template v-if="show.address">
              <th><span class="label">Address</span>
                <img v-on:click="sort('address')" src="../assets/icon-sort.png" />
                <img v-on:click="hide('address')" src="../assets/icon-hide.png" />
              </th>
            </template>
            <template v-if="show.contact">
              <th><span class="label">Primary Contact</span>
                <img v-on:click="sort('contact')" src="../assets/icon-sort.png" />
                <img v-on:click="hide('contact')" src="../assets/icon-hide.png" />
              </th>
            </template>
            <template v-if="show.level">
              <th><span class="label">Membership Level</span>
                <img v-on:click="sort('level')" src="../assets/icon-sort.png" />
                <img v-on:click="hide('level')" src="../assets/icon-hide.png" />
              </th>
            </template>
            <template v-if="show.purchases">
              <th><span class="label">Total Purchases ($)</span>
                <img v-on:click="sort('purchases')" src="../assets/icon-sort.png" />
                <img v-on:click="hide('purchases')" src="../assets/icon-hide.png" />
              </th>
            </template>
            <th> </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in filteredItems" :key="item.id" v-bind:value="item.level">
            <td v-if="show.name">{{ item.name }}</td>
            <td v-if="show.address">{{ item.address }}</td>
            <td v-if="show.contact">{{ item.contact }}</td>
            <td v-if="show.level">{{ item.level }}</td>
            <td v-if="show.purchases">{{ item.purchases }}</td>
            <td>
              <img v-on:click="remove(item)" src="../assets/delete.png" />
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
  let items = []
  items.push({
    key: 1,
    name: 'C Person',
    address: '457 This Place, Suburbia, Auckland',
    contact: '011 123 4567',
    level: 'Gold',
    purchases: '20000'})
  items.push({
    key: 2,
    name: 'A Person',
    address: '123 That Place, Suburbia, Auckland',
    contact: '015 127 4568',
    level: 'Silver',
    purchases: '10000'})
  items.push({
    key: 3,
    name: 'B Person',
    address: '234 Other Place, Suburbia, Auckland',
    contact: '055 987 6543',
    level: 'Standard',
    purchases: '500'})
  items.push({
    key: 4,
    name: 'CD Person',
    address: '6234 Great Road, Suburbia, Auckland',
    contact: 'cd@great.com',
    level: 'Silver',
    purchases: '5000'})

const vm = {
    name: 'grid',
    data () {
      return {
        msg: 'Data Grid for Naveya & Sloane',
        items: items,
        show: {
          name: true,
          address: true,
          contact: true,
          level: true,
          purchases: true
        },
        selected: '',
        searchTerm: ''
      }
    },
    computed: {
      filteredItems: function () {
        const byLevelItems = this.itemsByLevel(this.items)
        return this.itemsByName(byLevelItems)
      }
    },
    methods: {
      sort: function (field) {
        items.sort((i1, i2) => i1[field].localeCompare(i2[field]))
      },
      remove: function (item) {
        for (let i = 0; i < items.length; i++) {
          if (items[i].key === item.key) {
            items.splice(i, 1)
            break
          }
        }
      },
      itemsByLevel: function (items) {
        const _level = this.selected
        return items.filter(function (i) {
          return i.level === _level || _level === ''
        })
      },
      itemsByName: function (items) {
        const _term = this.searchTerm.toLowerCase()
        return items.filter(function (i) {
          return i.name.toLowerCase().startsWith(_term) || _term === ''
        })
      },
      hide: function (column) {
        this.show[column] = false
      }
    }
}

export default vm
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
body {
  padding: 0;
  margin: 0;
}

h1, h2 {
  font-weight: normal;
}

table {
  width: 100%;
}

table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
  padding: 5px;
}

  th{
    background-color: lightgrey;
    text-align: left;
    font-size: 0.8em;

  }

  img {
    width: 25px;
    display: inline-block;
  }

  .filters {
    width: 50%;
  }

  .filter {
    text-align: left;
    padding-bottom: 10px;
  }
  .label {
    width: 80%;
    display: inline-block;
  }
  .labelName{

  }
  .name {
    font-size: 1em;
  }
  .level {
    font-size: 1em;
  }
</style>
