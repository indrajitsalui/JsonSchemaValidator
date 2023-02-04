<template>
  <div>
    <v-container>
      <v-row no-gutters>
        <v-col cols="12" md="5" class="ma-auto">
          <v-card class="pa-2" outlined tile>
            <CodeViewer :code="jsonSchema" />
          </v-card>
        </v-col>
        <v-spacer></v-spacer>
        <v-col cols="12" md="5" class="ma-auto">
          <v-card class="pa-2" outlined tile>
            <CodeViewer :code="jsonInput" />
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <span>Valid = {{ valid }}</span>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import CodeViewer from "../components/CodeViewer.vue";
// import {Validator} from "jsonschema";
export default {
  components: { CodeViewer },
  data: () => ({
    jsonSchema: {
      value: "{\n    \"type\": \"object\",\n    \"properties\": {\n        \"name\": { \"type\": \"string\" },\n        \"sku\": { \"type\": \"string\" },\n        \"price\": { \"type\": \"number\", \"minimum\": 0 },\n        \"shipTo\": {\n            \"type\": \"object\",\n            \"properties\": {\n                \"name\": { \"type\": \"string\" },\n                \"address\": { \"type\": \"string\" },\n                \"city\": { \"type\": \"string\" },\n                \"state\": { \"type\": \"string\" },\n                \"zip\": { \"type\": \"string\" }\n                }\n        },\n        \"billTo\": {\n            \"type\": \"object\",\n            \"properties\": {\n                \"name\": { \"type\": \"string\" },\n                \"address\": { \"type\": \"string\" },\n                \"city\": { \"type\": \"string\" },\n                \"state\": { \"type\": \"string\" },\n                \"zip\": { \"type\": \"string\" }\n                }\n        }\n    }\n}",
    },
    jsonInput: {
      value: "{ \"name\"   : \"John Smith\",\n  \"sku\"    : \"20223\",\n  \"price\"  : 23.95,\n  \"shipTo\" : { \"name\" : \"Jane Smith\",\n               \"address\" : \"123 Maple Street\",\n               \"city\" : \"Pretendville\",\n               \"state\" : \"NY\",\n               \"zip\"   : \"12345\" },\n  \"billTo\" : { \"name\" : \"John Smith\",\n               \"address\" : \"123 Maple Street\",\n               \"city\" : \"Pretendville\",\n               \"state\" : \"NY\",\n               \"zip\"   : \"12345\" }\n}",
    },
    valid: true,
  }),
  methods: {
    validateJsonInput(jsonSchemaInput, jsonInput) {
      try {
        jsonSchemaInput = JSON.parse(jsonSchemaInput);
        jsonInput = JSON.parse(jsonInput);
      } catch(e) {
        this.valid = false;
        console.log("failed parsed: jsonSchemaInput or jsonInput ", e);
        return;
      }
      console.log("Success")
      // this.valid = true
      this.valid = this.validateInputWithSchema(jsonSchemaInput,jsonInput);
    },
    validateInputWithSchema(jsonSchemaInput,jsonInput)
    {
      var Validator = require('jsonschema').Validator;
      var v = new Validator();
      var isValid = false
      try {
        isValid = v.validate(jsonInput, jsonSchemaInput).valid;
        return isValid
      }
      catch(e){
        console.log(e)
      }
      return isValid

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

<style scoped></style>
