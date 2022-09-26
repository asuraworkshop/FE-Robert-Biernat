<script>
export default {
  data() {
    return {
      name: '',
      habitability: 50,
      oxygenPercent: 50,
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
      animals: '',
      allAnimals: [
        { text: 'Mammals', value: 1 },
        { text: 'Birds', value: 2 },
        { text: 'Fish', value: 3 },
        { text: 'Reptiles', value: 4 },
        { text: 'Amphibians', value: 5 },
        { text: 'Arthropods', value: 6 }
      ],
      plants: '',
      allPlants: [
        { text: 'Alpines', value: 1 },
        { text: 'Annuals', value: 2 },
        { text: 'Aquatic plants', value: 3 },
        { text: 'Bulbs', value: 4 },
        { text: 'Cacti', value: 5 },
        { text: 'Carnivorous plants', value: 6 },
        { text: 'Climbers', value: 7 },
        { text: 'Ferns', value: 8 },
      ],
    }
  }
}

</script>

<template>
  <section>
    <h1>Planet Adder</h1>
    <form class="planet-form">

      <div class="planet-form_field">
        <label>Name:</label>
        <input v-model="name" placeholder="Planet name" />
      </div>

      <div class="planet-form_field">
        <label>Oxygen percent: ({{oxygenPercent}}%)</label>
        <input type="range" v-model="oxygenPercent"
         min="0" max="100" step="1">
      </div>

      <div class="planet-form_field">
        <label>Habitability: ({{habitability}}%)</label>
        <input type="range" v-model="habitability"
         min="0" max="100" step="1">
      </div>

      <div class="planet-form_field">
        <label class="label">Type:</label>
        <select v-model="type">
          <option v-for="option in allTypes" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>

      <div class="planet-form_field">
        <label class="label">Animals:</label>
        <select v-model="animals" multiple>
          <option v-for="option in allAnimals" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>

      <div class="planet-form_field">
        <label class="label">Plants:</label>
        <select v-model="plants" multiple>
          <option v-for="option in allPlants" :value="option.value">
            {{ option.text }}
          </option>
        </select>
      </div>

      <div class="planet-form_submit">
        <button type="submit">Add new planet</button>
      </div>

    </form>
  </section>
</template>
