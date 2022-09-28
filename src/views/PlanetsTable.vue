<script>
import axios from "axios";

export default {
  data() {
    return {
      planets: [],
      allRows: 0,
      pageList: [],
      planets_show: [],
      sort_column: 'id',
      sort_order: 'asc',
      start_row: 0,
      limit: 40,
      order_list: {
        name: 'asc',
        type: 'asc',
        habitability: 'asc',
        oxygen_percent: 'asc',
        distance: 'asc',
        discovery_date: 'asc'
      },
    };
  },
  async created() {
    try {
      const res = await axios.get('http://localhost:3000/planets');
      this.planets = res.data;
      this.allRows = this.planets.length;
      this.limitTable();
    } catch (error) {
      console.log(error);
    }
  },
  methods: {
    async sortTable(column) {
      this.sort_column = column;
      this.sort_order = this.order_list[column];
      if (this.order_list[column] == 'desc') {
        this.order_list[column] = 'asc';
      }
      else {
        this.order_list[column] = 'desc';
      }
    },
    async limitTable() {
      this.planets_show = [];
      var i = parseInt(this.start_row);
      var maxi = parseInt(this.start_row) + parseInt(this.limit);
      if (maxi > this.allRows) {
        maxi = this.allRows;
      }
      for (i = this.start_row; i < maxi; i++) {
        this.planets_show.push(this.planets[i]);
      }
      console.log(this.start_row);
      this.drawPages();
    },
    async drawPages() {
      this.pageList = [];
      var pages = Math.ceil(parseInt(this.allRows) / parseInt(this.limit));
      for (var i = 0; i < pages; i++) {
        var option = { text: i, value: i * this.limit }
        this.pageList.push(option);
      }
    },
    async deletePlanet(id) {
      let text = 'Are you sure to delete planet of ID: ' + id + '?';
      if (confirm(text) == true) {
        try {
          await axios.delete('http://localhost:3000/planets/'+id);
          const res = await axios.get('http://localhost:3000/planets');
          this.planets = res.data;
          this.allRows = this.planets.length;
          this.limitTable();
        } catch (error) {
          console.log(error);
        }
      }
    }
  }
};
</script>

<template>
  <section>
    <h1>Planet List</h1>
    <form class="planet-form" @submit.prevent="limitTable()">

      <div class="planet-form_field">
        <label>Rows per page:</label>
        <input v-model="limit" placeholder="Rows per page" type="text" maxlength="24" @change="limitTable()">
      </div>

      <div class="planet-form_field">
        <label class="label">Page:</label>
        <select v-model="start_row" @change="limitTable()">
          <option v-for="option in pageList" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>

    </form>

    <table class="planet-list">
      <tr>
        <th @click="sortTable('name')">Name</th>
        <th @click="sortTable('type')">Type</th>
        <th @click="sortTable('habitability')">Habitability</th>
        <th @click="sortTable('oxygen_percent')">Oxygen percent</th>
        <th @click="sortTable('distance')">Distance from Earth</th>
        <th @click="sortTable('discovery_date')">Discovery Date</th>
        <th></th>
      </tr>
      <tr v-for="planet of planets_show" :key="planet.id">
        <td>{{ planet.name }}</td>
        <td>{{ planet.type }}</td>
        <td>{{ planet.habitability }}</td>
        <td>{{ planet.oxygen_percent }}%</td>
        <td>{{ planet.distance }} ly</td>
        <td>{{ planet.discovery_date }}</td>
        <td><svg @click="deletePlanet(planet.id)" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 48 48"><path d="M13.05 42q-1.25 0-2.125-.875T10.05 39V10.5H8v-3h9.4V6h13.2v1.5H40v3h-2.05V39q0 1.2-.9 2.1-.9.9-2.1.9Zm21.9-31.5h-21.9V39h21.9Zm-16.6 24.2h3V14.75h-3Zm8.3 0h3V14.75h-3Zm-13.6-24.2V39Z"/></svg></td>
      </tr>
    </table>

  </section>
</template>
