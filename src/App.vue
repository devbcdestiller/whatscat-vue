<script setup>
import { ref } from 'vue'
import FormComponent from './components/FormComponent.vue'
import ResultsComponent from './components/ResultsComponent.vue'
import NavbarComponent from './components/NavbarComponent.vue'

const apiURL = "https://whatscat-api.ml/v1/predict";

let breeds = [];
let isDone = ref(false);
let isUploaded = ref(true);
let child = ref(null);
let b64Img = ref('');

function uploadImg() {
  if (!child.value['isUploaded']) {
    isUploaded.value = false;
    return
  }

  let formData = new FormData();
  let imgFile = child.value['img']['inputImg'];
  b64Img.value = child.value['b64Img'];
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
      percentage: (predictions[i][0] * 100).toFixed(2)
    });
  }
  isDone.value = !isDone.value;
}

function tryAgain() {
  location.reload()
}

</script>

<template>
  <NavbarComponent />

  <div v-if="!isDone" class="grid-container">
    <div>
      <FormComponent ref="child" />
    </div>
    <div>
      <p v-if="!isUploaded">Please upload an image!</p>
      <button @click="uploadImg">Upload Image</button>
    </div>
  </div>

  <div v-if="isDone" class="grid-container">
    <ResultsComponent :results="breeds" :b64Img="b64Img"/>
    <div>
      <button @click="tryAgain">Try Again</button>
    </div>
  </div>

</template>

<style>
body {
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
  padding: 0;
}

p {
  text-align: center;
  color: gray;
  font-size: 1em;
  font-family: Arial, Helvetica, sans-serif;
}

h1 {
  text-align: center;
  color: gray;
  font-size: 1.5em;
  font-family: Arial, Helvetica, sans-serif;
}

.grid-container {
  margin: max(10vw);
  display: grid;
  grid-template-columns: 700px;
  justify-content: center;
  align-content: center;
}

.grid-container > div {
  justify-self: center;
}

button {
  width: max(20vw, 250px);
  height: max(2.5vw, 35px);
  font-size: max(1.25vw, 1em);
  border: none;
  background-color: #6fa9c5;
  color: white;
  border-radius: 1vw;
  margin: 1em;

}

button:enabled:hover {
  background-color: white;
  color: black;
  cursor: pointer;
  border: 2px solid #6fa9c5;
  transition-duration: 0.3s;
}

button:disabled {
  background-color: gray;
}

a {
  text-decoration: none;
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #333;
}

li {
  float: left;
}

li a {
  display: block;
  color: white;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
</style>