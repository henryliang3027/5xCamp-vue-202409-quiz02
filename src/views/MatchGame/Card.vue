<script setup>
const props = defineProps({
  idx: {
    type: Number,
    required: true,
  },
  n: {
    type: Number,
    required: true,
  },
  isOpen: {
    type: Boolean,
    default: false,
  },
});

const emit = defineEmits(["cardClicked"]);

// Handle click events
const clickHandler = () => {
  if (!props.isOpen) {
    emit("cardClicked", props.id);
  }
};
</script>

<template>
  <div
    :key="idx"
    class="flip-card"
    :class="{
      open: isOpen,
    }"
    @click="clickHandler"
  >
    <div class="flip-card-inner" v-if="n > 0">
      <div class="flip-card-front">{{ n }}</div>
      <div class="flip-card-back">
        <img :src="`./img/cat-0${n}.jpg`" alt="" />
      </div>
    </div>
  </div>
</template>

<style scoped src="./MatchGame.css"></style>
