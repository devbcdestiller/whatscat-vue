<script setup>
import { ref, reactive, defineExpose} from 'vue'

let img = reactive({inputImg: {}});
const isUploaded = ref(false);

const b64Img = ref('');
const fileReader = new FileReader();
defineExpose({
  img,
  isUploaded
})

function onInput(e) {
  img.inputImg = e.target.files[0];

  isUploaded.value = !isUploaded.value;
  fileReader.readAsDataURL(e.target.files[0])
  fileReader.onload = function() {
    b64Img.value = fileReader.result;
  };
}



</script>

<template>
  <img v-if="isUploaded" :src="b64Img" alt="" accept="*/image" width="640" height="360">
  <input type="file" @change="onInput" placeholder="Type here">
</template>