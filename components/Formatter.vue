<template>
  <div>
    <v-container>
      <v-row no-gutters>
        <v-col cols="12" md="6" class="pr-5">
          <v-card class="jsonInputCard" outlined tile>
            <div
              style="background: #132122; color: #e8ba55"
              class="text-center"
            >
              <span> Enter Json input below</span>
            </div>
            <CodeViewer :code="jsonInput" />

            <!-- <span style="background: #132122">Enter Json schema below</span>  -->
          </v-card>
        </v-col>

        <v-col cols="12" md="6" class="pl-5">
          <v-card class="jsonInputCard" outlined tile>
            <div
              style="background: #132122; color: #e8ba55"
              class="text-center"
            >
              <span> Formatted Json</span>
            </div>
            <CodeViewer :code="jsonFormattedOutput" />
          </v-card>
        </v-col>
      </v-row>
      <v-row>
        <v-col cols="12" class="ma-auto">
          <v-card class="pa-2" outlined tile color="codeEditor" height="15vh">
            <div v-if="!inputValid">
              <v-icon style="color: #ff5722"> mdi-alert-circle-outline</v-icon>
              <span class="body-1" style="color: #ff5722">
                Invalid Json Input: {{errorMessage}}
              </span>
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
    jsonInput: {
      value: 
      '{ "name"   : "John Smith",\n  "sku"    : "20223",\n  "price"  : 23.95,\n  "shipTo" : { "name" : "Jane Smith",\n               "address" : "123 Maple Street",\n               "city" : "Pretendville",\n               "state" : "NY",\n               "zip"   : "12345" },\n  "billTo" : { "name" : "John Smith",\n               "address" : "123 Maple Street",\n               "city" : "Pretendville",\n               "state" : "NY",\n               "zip"   : "12345" }\n}',

    },
    jsonFormattedOutput: {
      value: null
    },
    inputValid: true,
    errorMessage: ''

  }),
  methods: {
    validateJsonInput(jsonInput) {
      try {
        jsonInput = JSON.parse(jsonInput);
        this.inputValid = true;
      } catch (e) {
        this.inputValid = false;
        this.errorMessage = e.message;
        return false;
      }
      return true
    }
  },
  watch: {
    "jsonInput.value": {
      handler(jsonInput) {

        if(this.validateJsonInput(jsonInput))
        {
          this.jsonFormattedOutput.value = JSON.stringify(JSON.parse(jsonInput), null, 4); 
        }
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
