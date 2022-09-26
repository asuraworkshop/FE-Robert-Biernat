<script setup>
import { RouterLink, RouterView } from 'vue-router'
</script>

<template>

  <header class="header-main">
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />
    <nav>
      <RouterLink to="/">Formularz</RouterLink>
      <RouterLink to="/planet-table">Tabela</RouterLink>
    </nav>
  </header>

  <RouterView />

</template>

<style scoped>



</style>
