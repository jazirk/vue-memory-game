<template>
  <header><em>Memory Game</em></header>
  <div class="container">
    <Button v-if="!start" class="btn-start" @click="start = true">Start</Button>
    <Block
      v-else
      v-for="(number, index) in randomNumber"
      :key="index"
      :value="number"
      :disabled="disabled[index]"
      :disableClick="disableClick"
      @onClick="onBlockClick"
    />
  </div>
</template>

<script setup>
import Block from "./components/Block.vue";
import { computed, ref, watch } from "vue";

const start = ref(false);

const num1 = ref(null);
const num2 = ref(null);

const disableClick = ref(false);

const disabled = ref(new Array(18).fill(false));

const randomNumber = computed(() => {
  const nums1 = new Set();
  while (nums1.size !== 18) {
    nums1.add(Math.floor(Math.random() * 18) + 1);
  }

  const nums2 = new Set();
  while (nums2.size !== 18) {
    nums2.add(Math.floor(Math.random() * 18) + 1);
  }

  console.log(nums1, nums2);
  return [...nums1, ...nums2];
});

const onBlockClick = (val) => {
  if (!num1.value && !num2.value) {
    num1.value = val;
  } else if (!num2.value) {
    num2.value = val;
    disableClick.value = true;
    let timer = setTimeout(() => {
      console.log(num1.value);

      if (num1.value === num2.value) {
        randomNumber.value.forEach((num, index) => {
          if (num === num1.value || num === num2.value) {
            disabled.value[index] = true;
          }
        });
      }
      num1.value = null;
      num2.value = null;
      disableClick.value = false;
      clearTimeout(timer);
    }, 1500);
  } else {
    //
  }
};

watch("disabled", (val) => {
  if (val.every((v) => v)) {
    start.value = false;
  }
});
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.container {
  padding: 10px;
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  width: 80%;
  height: 80%;
  place-items: center;
  gap: 5px;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  margin: auto;
}
header {
  font-size: 2em;
}
.btn-start {
  background-color: #37b386;
  color: white;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  position: absolute;
  height: 100px;
  font-size: 2em;
  cursor: pointer;
  width: 100px;
  border-radius: 10px;
}
</style>
