<template>
  <Transition>
    <div
      :class="[{ block: true, disable: disableClick, matched: card?.matched }]"
      @click="onBlockClick"
    >
      {{ checkedNumber || "" }}
    </div>
  </Transition>
</template>

<script setup>
import { ref, watch } from "vue";
// eslint-disable-next-line no-undef
const props = defineProps({
  card: {
    type: Object,
  },
  disabled: {
    type: Boolean,
  },
  disableClick: {
    type: Boolean,
  },
});
// eslint-disable-next-line no-undef
const emits = defineEmits(["onClick"]);

const checkedNumber = ref(null);

console.log(props.card);
const onBlockClick = () => {
  checkedNumber.value = props.card?.value;
  emits("onClick", props.card);
};

watch(
  () => props.disableClick,
  (val) => {
    console.log(val);
    if (!val) {
      checkedNumber.value = 0;
    }
  }
);
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.block {
  font-size: 1.5rem;
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100px;
  width: 100px;
  background-color: #b3ad37;
  color: white;
  cursor: pointer;
}

.disable {
  cursor: not-allowed;
  pointer-events: none;
}

.matched {
  visibility: hidden;
}

/* we will explain what these classes do next! */
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
