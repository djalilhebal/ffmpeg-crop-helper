<script setup>
import { ref } from 'vue';

const objectUrls = new Set();

// Template refs
const videoEl = ref(null);
const imgEl = ref(null);
const rectStyle = ref(`
stroke-width: 2px;
stroke: yellow;
`.trim());

// Input
const objectUrl = ref('');
const objectType = ref('');
const objectWidth = ref(0);
const objectHeight = ref(0);

// Output
const w = ref(100);
const h = ref(100);
const x = ref(0);
const y = ref(0);

function updateCropParams() {
  w.value = objectWidth.value;
  h.value = objectHeight.value;
  x.value = 0;
  y.value = 0;
}

function handleFile(ev) {
  const file = ev.target.files[0];
  // 'video/mp4' -> 'video'
  // 'image/jpeg' -> 'image'
  const fileType = file.type.split('/')[0];
  const fileObjectUrl = URL.createObjectURL(file);

  // TODO: Clean previous obj urls
  objectUrls.add(fileObjectUrl);

  objectUrl.value = fileObjectUrl;
  objectType.value = fileType;
}

function updateVideoDims() {
  objectWidth.value = videoEl.value.videoWidth;
  objectHeight.value = videoEl.value.videoHeight;

  updateCropParams();
}

function updateImageDims() {
  objectWidth.value = imgEl.value.naturalWidth;
  objectHeight.value = imgEl.value.naturalHeight;

  updateCropParams();
}
</script>

<template>
  <main>
    <form class="kai-controls bg-purple-900 text-white p-2">
      <div class="inline-block">
        <b>Original:</b>

        <label>
          File
          <input type="file" accept="image/*,video/*" @change="handleFile" />
        </label>
      </div>

      <div class="inline-block">
      <b>Crop mask:</b>

        <label>
          Width
          <input v-model="w" type="number" min="0" :max="objectWidth" />
        </label>
        <label>
          Height
          <input v-model="h" type="number" min="0" :max="objectHeight" />
        </label>
        <label>
          X
          <input v-model="x" type="number" min="0" :max="objectWidth" />
        </label>
        <label>
          Y
          <input v-model="y" type="number" min="0" :max="objectHeight" />
        </label>
        <label>
          Style
          <textarea v-model="rectStyle" autocapitalize="off" spellcheck="false" class="whitespace-pre-line text-sm font-mono"></textarea>
        </label>
      </div>
    </form>

    <p v-if="!objectUrl" class="text-2xl font-bold text-purple-950">
      To start, select an image or video!
    </p>

    <svg v-if="objectUrl" :viewBox="`0 0 ${objectWidth} ${objectHeight}`">
      <foreignObject :width="objectWidth" :height="objectHeight" class="crop-object">
        <video controls="controls" ref="videoEl" v-if="objectType === 'video'" :src="objectUrl"
          @loadedmetadata="updateVideoDims"></video>
        <img ref="imgEl" v-if="objectType === 'image'" :src="objectUrl" @load="updateImageDims" />
      </foreignObject>

      <rect class="crop-mask" :style="rectStyle" :width="w" :height="h" :x="x" :y="y" />
    </svg>

    <output v-if="objectUrl">
      Command to copy: <br />
      <code class="font-monos">
        ffmpeg -i input.xyz -filter:v "<b class="text-purple-950">crop={{ w }}:{{ h }}:{{ x }}:{{ y }}</b>" output.xyz
      </code>
    </output>
  </main>
</template>

<style scoped>
main {
  text-align: center;
  flex-grow: 1;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

p {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
}

label {
  margin-right: 0.5rem;
}

input {
  text-align: center;
}

label > *:not([type=file]) {
  color: black;
}

svg {
  max-width: 100%;
  flex-grow: 1;
  flex-basis: 100px;
}

.crop-mask {
  fill: none;
  stroke-width: 2px;
  stroke: yellow;
}
</style>
