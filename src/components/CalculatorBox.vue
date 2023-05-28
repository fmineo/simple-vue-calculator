<template>
  <div class="calculator-box">
    <h1 class="col-span-4 text-white text-lg font-semibold -my-3">Vue.js Simple Calculator</h1>
    <div class="col-span-4 text-right text-white h-40 max-h-40 overflow-y-auto mt-1 flex flex-col-reverse">
      <div class="relative">
        <ResultDisplay :value="displayValue" />
        <span class="absolute right-0 bottom-1 text-lg">{{ resultPreview }}</span>
      </div>
      <ul class="history-list text-sm mb-2">
        <li v-for="(historyItem, index) in history" :key="index">{{ historyItem }}</li>
      </ul>
    </div>
    <GenericButton v-for="(button, index) in buttons" :key="button" :text="button" @click="handleButtonClick(button)"
      :isLast="index === buttons.length - 1" :isAction="isAction(button)" :isAc="index == 1" :isC="index == 0"
      :resultIsEmpty="displayValue == '0'" />
  </div>
</template>

<script>
import GenericButton from './GenericButton.vue'
import ResultDisplay from './ResultDisplay.vue'

export default {
  components: {
    GenericButton,
    ResultDisplay
  },
  data() {
    return {
      displayValue: '0',
      resultPreview: '',
      buttons: [
        'C',
        'AC',
        '⌫',
        '%',
        '÷',
        '7',
        '8',
        '9',
        '×',
        '4',
        '5',
        '6',
        '-',
        '1',
        '2',
        '3',
        '+',
        '0',
        ',',
        '='
      ],
      actions: [
        'C',
        'AC',
        '⌫',
        '÷',
        '×',
        //',',
        '-',
        '+',
        '%',
        '*',
        '/'
      ],
      operators: ['+', '-', '*', '/'],
      history: []
    }
  },
  methods: {
    handleButtonClick(button) {
      switch (button) {
        case 'AC':
          this.clearDisplayAndHistory()
          break
        case 'C':
          this.clearDisplay()
          break
        case '⌫':
          this.backSpace()
          break
        case '=':
          this.calculateResult()
          break
        case '÷':
          this.addOperator('/')
          break
        case '×':
          this.addOperator('*')
          break
        case ',':
          this.addDecimal()
          break
        case '%':
          this.calculatePercentage()
          break
        default:
          this.addToDisplay(button)
      }
    },
    clearDisplay() {
      this.displayValue = '0'
    },
    clearDisplayAndHistory() {
      this.displayValue = '0'
      this.clearHistory()
    },
    backSpace() {
      this.displayValue = this.displayValue.slice(0, -1)
      if (this.displayValue.length == 0) this.displayValue = '0'
    },
    addToDisplay(value) {
      if (this.displayValue == '0') {
        this.displayValue = value
      } else {
        this.displayValue += value
      }
    },
    addOperator(operator) {
      if (this.displayValue.length > 0) {
        const lastChar = this.displayValue.slice(-1)
        if (this.isOperator(lastChar)) {
          this.displayValue = this.displayValue.slice(0, -1) + operator
        } else {
          this.addToDisplay(operator)
        }
      }
    },
    addDecimal() {
      if (!this.displayValue.includes('.')) {
        this.addToDisplay('.')
      }
    },
    isOperator(value) {
      const operators = this.operators
      return operators.includes(value)
    },
    isAction(button) {
      return this.actions.includes(button)
    },
    calculateResult() {
      if (this.displayValue != '0') {
        try {
          const operation = this.displayValue
          const result = eval(operation)
          this.displayValue = String(result)
          this.addToHistory(operation)
          this.addToHistory('= ' + result)
        } catch (error) {
          // console.error('Error:', error);
        }
      }
    },
    calculatePercentage() {
      try {
        const regex = new RegExp(`(${this.actions.map((action) => '\\' + action).join('|')})`)

        const splitted = this.displayValue.split(regex).filter(Boolean)
        const value = parseFloat(splitted.slice(-1))
        const newValue = splitted.slice(0, splitted.length - 1)
        const newValueString = this.removeOperatorsFromEnd(String(newValue.join('')))
        const prevCalc = eval(newValueString)

        const percentage = (prevCalc / 100) * value

        if (!isNaN(percentage)) {
          newValue.push(String(percentage))
          this.displayValue = newValue.join('')
        }
      } catch (error) {
        // console.error("Errore", error);
      }
    },
    removeOperatorsFromEnd(string) {
      let result = string.trim()
      let lastIndex = result.length - 1

      while (lastIndex >= 0 && this.operators.includes(result[lastIndex])) {
        result = result.slice(0, lastIndex)
        lastIndex--
      }

      return result
    },

    parseExpression() {
      const expression = this.displayValue
      const regex = /(\d+(\.\d+)?)[+\-*/]?$/
      const matches = expression.match(regex)
      if (matches && matches.length > 1) {
        return parseFloat(matches[1])
      }
      return null
    },
    addToHistory(item) {
      this.history.push(item)
    },
    clearHistory() {
      this.history = []
    }
  },
  watch: {
    displayValue(displayValue) {
      if (displayValue != '0') {
        try {
          const result = eval(displayValue)
          this.resultPreview = '= ' + String(result)
        } catch (error) {
          // console.error('Error:', error);
        }
      } else {
        this.resultPreview = ''
      }
    }
  },
  created() {
    document.addEventListener("keydown", (event) => {
      const key = event.key;
      const validKeys = [
        '0', '1', '2', '3', '4', '5', '6', '7', '8', '9',
        '+', '-', '*', '/', '.', ',', '%', 'Enter', 'Backspace', 'Escape', 'Delete', '='
      ];

      if (validKeys.includes(key)) {
        event.preventDefault();

        if (key === 'Enter' || key === '=') {
          this.calculateResult();
        } else if (key === 'Backspace') {
          this.backSpace();
        } else if (key === 'Escape' || key === 'Delete') {
          this.clearDisplay();
        } else {
          this.handleButtonClick(key);
        }
      }
    });

  }
}
</script>

<style scoped>
.calculator-box {
  @apply border border-indigo-600 w-auto p-6 rounded-xl drop-shadow-2xl grid grid-cols-4 gap-4 mx-4 sm:mx-auto bg-gradient-to-br from-indigo-700 via-indigo-600 to-indigo-700;
}
</style>
