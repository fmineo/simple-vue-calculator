<script>
export default {
  props: {
    value: String
  }
}
</script>

<template>
  <input
    :class="['calculator-display', { 'pb-1': value == 0, 'pb-8': value != 0 }]"
    type="text"
    :value="value"
    disabled
    placeholder="0"
    
  />
</template>

<style scoped>
.calculator-display {
  @apply border-b border-b-rose-200 text-3xl text-right focus:outline-none bg-transparent text-white w-full;
}
</style>
