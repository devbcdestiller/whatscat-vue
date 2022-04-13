<script setup>
import { ref } from 'vue'
import FormComponent from './components/FormComponent.vue'
import ResultsComponent from './components/ResultsComponent.vue'

const apiURL = "https://whatscat-api.ml/v1/predict";


let breeds = [];
let isDone = ref(false);
let child = ref(null);

function uploadImg() {
  let formData = new FormData();
  let imgFile = child.value['img']['inputImg'];
  formData.append("img", imgFile, imgFile['name']);
  let requestOptions = {
    method: 'POST',
    body: formData
  };

  fetch(apiURL, requestOptions)
  .then(response => response.json())
  .then(result =>
    processResult(result)
  )
  .catch(error => console.log(error));

}

function processResult(rawData) {
  let predictions = rawData['predictions'];
  
  const numOfResult = 3;
  for (let i = 0; i < numOfResult; i++) {
    breeds.push({
      id: i + 1,
      name: predictions[i][1],
      percentage: predictions[i][0] * 100
    });
  }
  console.log(breeds)
  isDone.value = !isDone.value;
}

function tryAgain() {
  location.reload()
}

</script>

<template>
  <div v-if="!isDone">
    <FormComponent ref="child" />
    <button @click="uploadImg">Upload Image</button>
  </div>
  <div v-if="isDone">
    <ResultsComponent :results="breeds" />
    <button @click="tryAgain">Try Again</button>
  </div>
</template>