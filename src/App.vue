<template>
  <div class="calculator-conainer">
    <CalculatorBox />
  </div>
</template>

<script>
import CalculatorBox from './components/CalculatorBox.vue'

export default {
  components: {
    CalculatorBox
  },
  mounted() {
    const titleEl = document.querySelector('head title')
    titleEl.textContent = 'Vue.js Simple Calculator'

    const metaDescription = document.createElement('meta')
    metaDescription.setAttribute('name', 'description')
    metaDescription.setAttribute('content', 'Vue.js Simple Calculator')

    const headElement = document.querySelector('head')
    headElement.appendChild(metaDescription)
  }
}
</script>

<style scoped>
.calculator-conainer {
  @apply grid
    h-screen
    place-items-center;
}
</style>
