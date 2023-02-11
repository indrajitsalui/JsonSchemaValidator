<template>
  <div>
    <v-container >
      <v-row justify="end">
        <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
            <v-spacer/>
        <v-col justify="end">
        <v-btn @click="swap">
          <v-icon class ="title">
            mdi-invert-colors
          </v-icon>
        </v-btn>
      </v-col>
      </v-row>
      <v-row no-gutters>
        <v-col cols="12" md="6" class="pa-5 text-center"  >
          <v-card  class=" jsonInputCard" outlined tile>
           Enter the Json Schema below
            <CodeViewer :code="jsonSchema" />
          </v-card>
        </v-col>

          <v-col cols="12" md="6" class="pa-5 text-center" >
          <v-card  class=" jsonInputCard " outlined tile>
            Enter the Json input below
            <CodeViewer :code="jsonInput"  />
          </v-card>

        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" class="ma-auto">
          <v-card class="pa-2" outlined tile>
            Result : {{ errorMessage }}
            Errors : <div v-for="(item,index) in validatorResult.errors" :key="index">
              {{ item }}
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>

</template>

<script>
import CodeViewer from "../components/CodeViewer.vue";

export default {
  components: { CodeViewer },
  data: () => ({
    jsonSchema: {
      value:
        '{\n    "type": "object",\n    "properties": {\n        "name": { "type": "string" },\n        "sku": { "type": "string" },\n        "price": { "type": "number", "minimum": 0 },\n        "shipTo": {\n            "type": "object",\n            "properties": {\n                "name": { "type": "string" },\n                "address": { "type": "string" },\n                "city": { "type": "string" },\n                "state": { "type": "string" },\n                "zip": { "type": "string" }\n                }\n        },\n        "billTo": {\n            "type": "object",\n            "properties": {\n                "name": { "type": "string" },\n                "address": { "type": "string" },\n                "city": { "type": "string" },\n                "state": { "type": "string" },\n                "zip": { "type": "string" }\n                }\n        }\n    }\n}',
    },
    jsonInput: {
      value:
        '{ "name"   : "John Smith",\n  "sku"    : "20223",\n  "price"  : 23.95,\n  "shipTo" : { "name" : "Jane Smith",\n               "address" : "123 Maple Street",\n               "city" : "Pretendville",\n               "state" : "NY",\n               "zip"   : "12345" },\n  "billTo" : { "name" : "John Smith",\n               "address" : "123 Maple Street",\n               "city" : "Pretendville",\n               "state" : "NY",\n               "zip"   : "12345" }\n}',
    },
    valid: true,
    errorMessage: "",
    validatorResult:[],
  }),
  methods: {
    validateJsonInput(jsonSchemaInput, jsonInput) {
      try {
        jsonInput = JSON.parse(jsonInput);
      } catch (e) {
        this.valid = false;
        this.errorMessage = "Json Input Syntax Exception : " + e.message;
        return;
      }
      try {
        jsonSchemaInput = JSON.parse(jsonSchemaInput);
      } catch (e) {
        this.valid = false;
        this.errorMessage = "Json Schema Syntax Exception : " + e.message;
        return;
      }
      this.valid = this.validateInputWithSchema(jsonSchemaInput, jsonInput);
      this.errorMessage = this.valid
        ? "Json input is compatible with Json schema"
        : "Json input is not compatible with Json schema";
    },
    validateInputWithSchema(jsonSchemaInput, jsonInput) {
      var Validator = require("jsonschema").Validator;
      var v = new Validator();
      var isValid = false;
      try {
        this.validatorResult = v.validate(jsonInput, jsonSchemaInput);
        isValid=this.validatorResult.valid;
        return isValid;
      } catch (e) {
        this.errorMessage = e.message;
      }
      return isValid;
    },
    computed: {
    theme () {
      return this.$vuetify.theme.isDark ? 'dark' : 'light'
    }
  },
  touchAll () {
      const value = this.$vuetify.theme.themes[this.theme]
      this.$vuetify.theme.themes[this.theme] = {}
      this.$vuetify.theme.themes[this.theme] = value
    },

    swap () {
      this.$vuetify.theme.isDark = !this.$vuetify.theme.isDark

      // necessary to reset colors after changing the theme, perhaps a Vuetify.js bug
      this.touchAll(1)
    }

  },
  watch: {
    "jsonSchema.value": {
      handler(newValue) {
        this.validateJsonInput(newValue, this.jsonInput.value);
      },
      immediate: true,
    },
    "jsonInput.value": {
      handler(newValue) {
        this.validateJsonInput(this.jsonSchema.value, newValue);
      },
      immediate: true,
    },
  },
};
</script>

<style scoped>
.jsonInputCard{
  height: 70vh;
}


</style>
