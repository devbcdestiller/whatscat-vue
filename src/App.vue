<script setup>
import { ref } from 'vue'

let img = '';
const apiURL = 'https://whatscat-api.ml/v1/predict';
const b64Img = ref('')
const text = ref('');
const isUploaded = ref(false);
const formData = new FormData();
const fileReader = new FileReader();


function onInput(e) {
  img = e.target.files[0];
  console.log(img)
  isUploaded.value = !isUploaded.value;
  fileReader.readAsDataURL(img)
  fileReader.onload = function() {
    b64Img.value = fileReader.result;
  };

}

function confirmSubmit() {
  formData.append("img", img, img['name']);
  const requestOptions = {
    method: 'POST',
    body: formData
  };

  fetch(apiURL, requestOptions)
  .then(response => 
    response.text(),
    text.value = 'loading...'
  )
  .then(result =>
    text.value = result
  )
  .catch(error => console.log(error));
}
</script>

<template>
  <img v-if="isUploaded" :src="b64Img" alt="" accept="*/image" width="640" height="360">
  <input type="file" @change="onInput" placeholder="Type here">
  <button :disabled="!isUploaded" @click="confirmSubmit">Upload Image</button>
  
  <p v-if="isUploaded">{{ text }}</p>
  <p v-else>
    Upload Image
  </p>
</template>