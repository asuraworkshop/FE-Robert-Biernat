<script>
import axios from "axios";
export default {
  data() {
    return {
      planets: [],
    };
  },
  async created() {
    try {
      const res = await axios.get(`http://localhost:3000/planets`);
      this.planets = res.data;
    } catch (error) {
      console.log(error);
    }
  },
};
</script>

<template>
  <section>
    <h1>Planet List</h1>
    <table class="planet-list">
      <tr>
        <th>ID</th>
        <th>Name</th>
        <th>Type</th>
        <th>Habitability</th>
        <th>Oxygen percent</th>
        <th>Distance from Earth</th>
        <th>Discovery Date</th>
      </tr>
      <tr v-for="planet of planets" :key="planet.id">
        <td>{{ planet.id }}</td>
        <td>{{ planet.name }}</td>
        <td>{{ planet.type }}</td>
        <td>{{ planet.habitability }}</td>
        <td>{{ planet.oxygen_percent }}</td>
        <td>{{ planet.distance }}</td>
        <td>{{ planet.discovery_date }}</td>
      </tr>
    </table>
  </section>
</template>
