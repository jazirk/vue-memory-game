<template>
  <Transition>
    <div
      :class="[{ block: true, disable: disableClick, visbility: disabled }]"
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
  value: {
    type: Number,
    default: 0,
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

console.log(props.disableClick);
const checkedNumber = ref(0);

const onBlockClick = () => {
  checkedNumber.value = props.value;
  emits("onClick", checkedNumber.value, props.index);
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

.visbility {
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
