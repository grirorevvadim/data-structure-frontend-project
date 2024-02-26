<template>
  <v-container class="fill-height">
    <v-responsive class="align-center text-center fill-height">
      <div class="text-body-2 font-weight-light mb-n1">Welcome to</div>
      <h1 class="text-h2 font-weight-bold">TextPro App</h1>

      <div class="py-14" />
      <div>
        <v-row>
          <v-col class="d-flex flex-column" cols="9">
            <v-textarea
              v-model="attribute"
              auto-grow
              class="flex-grow-1"
            ></v-textarea>
          </v-col>
          <v-col class="d-flex flex-column" cols="3">
            <v-btn class="mb-2" block>Load Text</v-btn>
            <v-btn @click="getMyData" class="mb-2" block>Flesch Index</v-btn>
            <v-btn class="mb-2" block>Edit distance</v-btn>
            <v-btn @click="markovWindow = true" class="mb-2" block
              >Generate Markov Text</v-btn
            >
            <v-checkbox label="Spelling Suggestions"></v-checkbox>
            <v-checkbox label="Autocomplete"></v-checkbox>
          </v-col>
        </v-row>
      </div>
      <v-row>
        <v-col cols="4">
          <v-card :text="result" variant="tonal"></v-card>
        </v-col>
        <v-col cols="2">
          <v-btn @click="clearRes">Clear</v-btn>
        </v-col>
      </v-row>
    </v-responsive>
    <div class="text-center">
      <v-dialog width="50%" v-model="markovWindow">
        <v-card>
          <div style="height: 300px">
            <v-card-text>
              {{ generatedText }}
            </v-card-text>
          </div>
          <v-textarea rows="1" v-model="wordsAmount"></v-textarea>
          <v-card-actions>
            <v-btn color="primary" @click="generateText">Generate</v-btn>
            <v-btn color="primary"  @click="closeMarkovWindow"
              >Close Dialog</v-btn
            >
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";
const attribute = ref("");
const wordsAmount = ref(0);
const result = ref("");
const generatedText = ref("");
const dialog = ref(false);
const markovWindow = ref(false);
const openMarkovWindow = () => {
  markovWindow.value = true;
};
const closeMarkovWindow = ()=>{
  markovWindow.value = false;
  wordsAmount.value = 0;
  generatedText.value = "";
};
const getMyData = async (): Promise<void> => {
  try {
    const response = await axios.get(
      `http://localhost:8080/get-data/${attribute.value}`
    );
    result.value = response.data;
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};
const trainOnText = async (): Promise<void> => {
  try {
    const response = await axios.post(
      `http://localhost:8080/get-data/train/${attribute.value}`
    );
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};

const generateText = async (): Promise<void> => {
  trainOnText();
  try {
    const response = await axios.get(
      `http://localhost:8080/get-data/markov/${wordsAmount.value}`
    );
    generatedText.value = response.data;
    console.log(response);
  } catch (error) {
    console.log(error);
  }
};
const clearRes = () => {
  result.value = "";
};
</script>
