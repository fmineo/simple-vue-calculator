<script>
export default {
  props: {
    text: String,
    isLast: Boolean,
    isAction: Boolean,
    isC: Boolean,
    isAc: Boolean,
    resultIsEmpty: Boolean
  }
}
</script>

<template>
  <button
    :class="[
      'calculator-button',
      {
        'flex col-span-2 bg-pink-500 text-white rounded-full': isLast,
        'flex bg-none border-none text-white aspect-square rounded-full hover:bg-indigo-500':
          !isLast && !isAction,
        'flex bg-none border-none text-pink-500 aspect-square rounded-full hover:bg-indigo-500':
          isAction,
        hidden: (isC && resultIsEmpty) || (isAc && !resultIsEmpty)
      }
    ]"
  >
    {{ text }}
  </button>
</template>

<style scoped>
.calculator-button {
  @apply border justify-center items-center border-indigo-600 hover:bg-opacity-75 text-3xl;
}
</style>
