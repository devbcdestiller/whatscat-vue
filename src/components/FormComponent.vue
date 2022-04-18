<script setup>
import { ref, reactive, defineExpose} from 'vue'

let img = reactive({inputImg: {}});
let isUploaded = ref(false);
let isSupportedFormat = ref(true);

const b64Img = ref('');
const supportedFormat = ["image/jpeg", "image/png"];
const fileReader = new FileReader();
defineExpose({
  img,
  isUploaded,
  b64Img
})

function onInput(e) {
  img.inputImg = e.target.files[0];

  if (!supportedFormat.includes(img.inputImg?.type)) {
    isSupportedFormat.value = false;
    return;
  } 

  isSupportedFormat.value = true;
  isUploaded.value = !isUploaded.value;
  fileReader.readAsDataURL(e.target.files[0])
  fileReader.onload = function() {
    b64Img.value = fileReader.result;
  };
}



</script>

<template>
  <p class="image-label" v-if="!isSupportedFormat">File format not supported. Upload only jpeg or png images.</p>
  <div class="input-container">
    <div v-if="isUploaded">
      <img :src="b64Img" alt="" accept="image/*">
      <p class="image-label"> {{ img.inputImg['name'] }}</p>
    </div>
    <div v-if="!isUploaded">
      <label class="input-button">
        <input type="file" @change="onInput">
        <span>+ Select Image</span>
      </label>
    </div>
    </div>
</template>

<style scoped>
img {
  padding: 0;
  margin: 0;
  border-radius: 3vw;
  width: min(64vw, 640px);
  height: min(36vw, 360px);
}

input {
  display: none;
}

.image-label {
  color: gray;
  font-size: 1em;
  padding: 0;
  margin: 0;
}

.input-button, p {
  text-align: center;
  cursor: pointer;
  color: gray;
  font-size: max(1.75vw, 1.5em);
  font-family: Arial, Helvetica, sans-serif;
}

.input-button:hover {
  background-color: white;
  color: black;
  transition-duration: 0.3s;
} 

.input-container {
  display: flex;
  justify-content: center;
  align-items: center;
  width: min(64vw, 640px);
  height: min(36vw, 360px);
  padding: 2.5vw;
  border: 0.2em dashed gray;
  border-radius: 1vw;
}
</style>