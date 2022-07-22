<script setup lang="ts">
import CanvasPaint from "./components/CanvasPaint.vue";
import { ref } from "vue";
let mode = ref("mode1");

function switchmode() {
  mode.value === "mode1" ? (mode.value = "mode2") : (mode.value = "mode1");
}
</script>

<template>
  <div>
    <div class="btn_c">
      <div class="button" @click="switchmode">Switch mode</div>
    </div>

    <CanvasPaint v-if="mode === 'mode1'" />
    <div v-else>haha</div>
  </div>
</template>

<style scoped>
.btn_c {
  z-index: 1;
  width: 100vw;
  height: auto;
  display: flex;
  align-items: center;
  justify-content: center;
  position: absolute;
  top: 10px;
  left: 0;
}
.button {
  width: 100px;
  height: 30px;
  background-color: rgba(8, 238, 96, 0.7);
  text-align: center;
  border-radius: 10px;
  padding-top: 5px;
  cursor: pointer;
}
.button:hover {
  background-color: rgba(253, 64, 1, 0.7);
}
</style>
