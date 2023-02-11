<template>
  <div>
    <v-app>
      <v-app-bar app color="navbar" elevation="0">
        <v-app-bar-title class="logo">JSON Schema Validator</v-app-bar-title>
      </v-app-bar>

      <v-main><Nuxt /> </v-main>
    </v-app>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.logo {
  font-size: 30px;
  font-weight: bold;
  color: #212121;

}
.theme--light.v-application {
  background-color: var(--v-background-base, #eddcd9) !important;
}
</style>
