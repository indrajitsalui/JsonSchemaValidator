<template>
  <v-app>
    <v-app-bar app color="navbar" elevation="0">
      <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
          <v-btn color="blue-grey" dark v-bind="attrs" v-on="on">
            <v-icon left dark> mdi-menu </v-icon>
            {{ selectedTool }}
          </v-btn>
        </template>
        <v-list>
          <v-list-item
            v-for="(item, index) in tools"
            :key="index"
            link
            @click="menuActionClick(item)"
          >
            <v-list-item-title>{{ item.title }}</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
      <!-- <v-app-bar-title class="logo">JSON {{ selectedTool }}</v-app-bar-title> -->
      
    </v-app-bar>
    <v-main> 
      <SchemaValidator v-if="selectedTool == 'Schema Validator'"/>
      <Formatter v-else-if="selectedTool == 'Formatter'"/>
      <Minifier v-else-if="selectedTool == 'Minify'"/>
      <Stringify v-else-if="selectedTool == 'Stringify'"/>
      <h1 v-else>No such tool</h1>
    </v-main>
  </v-app>
</template>

<script>
import SchemaValidator from "../components/SchemaValidator.vue";
import Minifier from "../components/Minifier.vue";
import Formatter from "../components/Formatter.vue";
import Stringify from "../components/Stringify.vue";

export default {
  components:{
    SchemaValidator,
    Formatter,
    Minifier,
    Stringify,
  },
  data: () => ({
    tools: [
      { title: "Formatter" },
      { title: "Minify" },
      { title: "Stringify" },
      { title: "Schema Validator" },
    ],
    selectedTool: "Formatter",
  }),
  methods: {
    menuActionClick(selectedItem) {
      this.selectedTool = selectedItem.title;
      //   console.log();
    },
  },
};
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
