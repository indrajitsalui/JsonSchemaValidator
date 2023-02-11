<template>
  <div>
    <v-app>
      <v-app-bar app>
        <v-spacer />
        JSON Schema Validator
        <v-spacer />
      </v-app-bar>

      <v-main><Nuxt /> </v-main>
    </v-app>
  </div>
</template>

<script>

export default{



}

;
</script>

<style scoped>
.theme--dark.v-application {
  background-color: var(--v-background-base, #18bc9c) !important;
}
.theme--light.v-application {
  background-color: var(--v-background-base, #93c1ec) !important;
}
</style>
