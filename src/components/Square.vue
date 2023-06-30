<script setup>
const props = defineProps(['value', 'winner', 'index'])
const emit = defineEmits(['setSquareValue'])

const handleClick = () => {
  emit('setSquareValue', props.index)
}
</script>

<template>
  <div v-if="value">
    <button :class="[`square square_${value.toLocaleLowerCase()}`]" disabled>
      {{ value }}
    </button>
  </div>
  <div v-else>
    <button class="square" :disabled="winner" @click="handleClick"></button>
  </div>
</template>

<style scoped>
.square {
  width: 100px;
  height: 100px;
  font-size: 5rem;
  color: white;
  border: solid 1px black;
  cursor: pointer;
}

.square_o {
  background-color: green;
}

.square_x {
  background-color: red;
}
</style>
