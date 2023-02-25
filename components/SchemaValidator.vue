<template>
  <div>
    <v-container>
      <v-row no-gutters>
        <v-col cols="12" md="6" class="pr-5" >
          <v-card class="jsonInputCard" outlined tile>
           
            <div style="background: #132122; color: #e8ba55" class="text-center" >
              <span> Enter Json schema below</span>
            </div>
            <CodeViewer :code="jsonSchema" />
           
            <!-- <span style="background: #132122">Enter Json schema below</span>  -->
            
          </v-card>
        </v-col>

        <v-col cols="12" md="6" class="pl-5">
          <v-card class="jsonInputCard" outlined tile>
            <div style="background: #132122; color: #e8ba55" class="text-center" >
              <span> Enter Json input below</span>
            </div>
            <CodeViewer :code="jsonInput" />
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" class="ma-auto">
          <v-card class="pa-2" outlined tile color="codeEditor" height="15vh">
            <div v-if="valid" style="color: #B9F6CA" >
              <v-icon style="color: #B9F6CA"  > mdi-check-circle-outline </v-icon>
              <span class="body-1">
                Provided Json input is compatible with provided Json-Schema
              </span>
            </div>
            
            <div v-else>
              <v-icon style="color: #FF5722" > mdi-alert-circle-outline</v-icon>
              <span class="body-1"  style="color: #FF5722" >
                Provided Json input is not compatible with provided Json-Schema
              </span>
              <!-- <br/> -->
              <p>
                <div v-for="(item, index) in validatorResult.errors" :key="index">
                  <span class="body-1" style="color: #FF8A80">
                    > {{ item }}
                  </span>
                  
                </div>
              </p>
              <!-- Result : {{ errorMessage }} -->
            </div>
          </v-card>
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import CodeViewer from "./CodeViewer.vue";

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
    validatorResult: [],
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
        isValid = this.validatorResult.valid;
        return isValid;
      } catch (e) {
        this.errorMessage = e.message;
      }
      return isValid;
    },
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
.jsonInputCard {
  height: 70vh;
}
/* .errorBoard{
  font-family: Fira code, Fira Mono, Consolas, Menlo, Courier, monospace;
} */
</style>
