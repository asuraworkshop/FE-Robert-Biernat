<script>
import axios from "axios";

export default {
  data() {
    return {
      name: '',
      habitability: true,
      oxygen_percent: 50,
      type: 'Volcanic',
      allTypes: [
        { text: 'Volcanic', value: 'Volcanic' },
        { text: 'Marsh', value: 'Marsh' },
        { text: 'Irradiated', value: 'Irradiated' },
        { text: 'Toxic', value: 'Toxic' },
        { text: 'Frozen', value: 'Frozen' },
        { text: 'Scorched', value: 'Scorched' },
        { text: 'Exotic', value: 'Exotic' },
        { text: 'Wet', value: 'Wet' },
      ],
      typeNames: ['Volcanic', 'Marsh', 'Irradiated', 'Toxic', 'Frozen', 'Scorched', 'Exotic', 'Wet'],
      discovery_date: new Date().toISOString().split('T')[0],
      distance: 10000,
      form_alert: 'Fill form...',
      errors: []
    }
  },
  methods: {
    async addItem() {
      this.form_alert = '...';
      this.errors = [];

      if (!this.name) {
        this.errors.push('Name required.');
      }
      if (this.name.length > 24 || this.name.length < 1) {
        this.errors.push('Name length is min 1 and max 24.');
      }

      if (this.habitability != true && this.habitability != false) {
        this.errors.push('Habitability incorrect value.');
      }

      if (!Number.isInteger(parseInt(this.oxygen_percent)) || this.oxygen_percent < 0 || this.oxygen_percent > 100) {
        this.errors.push('Oxygen percent incorrect value.');
      }

      if (!this.typeNames.includes(this.type)) {
        this.errors.push('Planet type not recognized.');
      }

      if (!Number.isInteger(this.distance) || this.distance < 1000 || this.distance > 100000) {
        this.errors.push('Distance incorrect value.');
      }

      if (isNaN(new Date(this.discovery_date))) {
          this.errors.push('Date incorrect format.');
      }

      if (this.errors.length === 0) {
        const res = await axios.post(`http://localhost:3000/planets`, {
          name: this.itemName,
          type: this.type,
          habitability: this.habitability,
          oxygen_percent: this.oxygen_percent,
          discovery_date: this.discovery_date,
        });
        this.form_alert = 'Planet Added';
        this.name = '';
        this.habitability = true;
        this.oxygen_percent = 50;
        this.type = 'Volcanic';
        this.discovery_date = new Date().toISOString().split('T')[0];
        this.distance = 10000;
      }
    }
  }
};

</script>

<template>
  <section>
    <h1>Planet Adder</h1>
    <form class="planet-form" @submit.prevent="addItem">

      <div class="planet-form_field">
        <label>Name:</label>
        <input v-model="name" placeholder="Planet name" type="text" maxlength="24" required>
      </div>

      <div class="planet-form_field">
        <label>Oxygen percent: ({{oxygen_percent}}%)</label>
        <input type="range" v-model="oxygen_percent"
         min="0" max="100" step="1" required>
      </div>

      <div class="planet-form_field">
        <label>Habitability:</label>
        <input type="checkbox" v-model="habitability">
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
        <ul>
          <li v-for="error in errors">{{ error }}</li>
        </ul>
      </div>

    </form>
  </section>
</template>
