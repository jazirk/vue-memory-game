<template>
  <header><em>Memory Game</em></header>
  <div class="container">
    <button v-if="!start" class="btn-start" @click="start = true">Start</button>
    <Block
      v-else
      v-for="(card, index) in randomNumbers"
      :key="index"
      :card="card"
      :disableClick="disableClick"
      @onClick="onBlockClick"
    />
  </div>
</template>

<script setup>
import Block from "./components/Block.vue";
import { computed, ref, watch } from "vue";

const start = ref(false);

const disableClick = ref(false);

const flippedCards = ref([]);

const randomNumbers = computed(() => {
  const nums1 = new Set();
  while (nums1.size !== 18) {
    nums1.add(Math.floor(Math.random() * 18) + 1);
  }

  const nums2 = new Set();
  while (nums2.size !== 18) {
    nums2.add(Math.floor(Math.random() * 18) + 1);
  }

  return [...nums1, ...nums2].map((num) => {
    return {
      value: num,
      matched: false,
    };
  });
});

const onBlockClick = (card) => {
  if (flippedCards.value.length < 2) flippedCards.value.push(card);
  if (flippedCards.value.length === 2) {
    disableClick.value = true;
    let timer = setTimeout(() => {
      if (flippedCards.value[0].value === flippedCards.value[1].value)
        flippedCards.value.forEach((_card) => (_card.matched = true));

      flippedCards.value = [];
      disableClick.value = false;
      clearTimeout(timer);
    }, 1500);
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
