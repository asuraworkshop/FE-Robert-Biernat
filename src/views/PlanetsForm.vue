<script>
import axios from "axios";

export default {
  data() {
    return {
      name: '',
      habitability: true,
      oxygen_percent: 50,
      type: 1,
      allTypes: [
      { text: 'Volcanic', value: 1 },
      { text: 'Marsh', value: 2 },
      { text: 'Irradiated', value: 3 },
      { text: 'Toxic', value: 4 },
      { text: 'Frozen', value: 5 },
      { text: 'Scorched', value: 6 },
      { text: 'Exotic', value: 7 },
      { text: 'Wet', value: 8 },
      ],
      discovery_date: new Date().toISOString().split('T')[0],
      distance: 1000,
      form_alert: '',
    }
  },
  methods: {
    async addItem() {
      this.form_alert = '...';
      const res = await axios.post(`http://localhost:3000/planets`, {
        name: this.itemName,
        type: this.type,
        habitability: this.habitability,
        oxygen_percent: this.oxygenPercent,
        discovery_date: this.discovery_date,
      });
      this.form_alert = 'Planet Added';
    },
  },
};

</script>

<template>
  <section>
    <h1>Planet Adder</h1>
    <form class="planet-form" @submit.prevent="addItem">

      <div class="planet-form_field">
        <label>Name:</label>
        <input v-model="name" placeholder="Planet name" required>
      </div>

      <div class="planet-form_field">
        <label>Oxygen percent: ({{oxygen_percent}}%)</label>
        <input type="range" v-model="oxygen_percent"
         min="0" max="100" step="1" required>
      </div>

      <div class="planet-form_field">
        <label>Habitability:</label>
        <div>
          <input type="checkbox" v-model="habitability" required>
        </div>
      </div>

      <div class="planet-form_field">
        <label class="label">Type:</label>
        <select v-model="type" required>
          <option v-for="option in allTypes" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>

      <div class="planet-form_field">
        <label class="label">Distance from Earth:</label>
        <input type="number" v-model="distance"
         min="1000" max="100000" step="1000" required>
      </div>

      <div class="planet-form_field">
        <label class="label">Discovery Date:</label>
        <input type="date" v-model="discovery_date" required>
      </div>

      <div class="planet-form_submit">
        <label class="label">Add new planet:</label>
        <button>Add new planet</button>
        <span class="planet-form_alert">{{form_alert}}</span>
      </div>

    </form>
  </section>
</template>
