<template>
  <div class="calculator">
    <div class="calcField rounded-t-lg">{{ current || '0' }}</div>
    <div class="btn !bg-[#3c3c3c]" @click="clear">C</div>
    <div class="btn !bg-[#3c3c3c]" @click="sign">+/-</div>
    <div class="btn !bg-[#3c3c3c]" @click="percent">%</div>
    <div class="btn operator" @click="divide">÷</div>
    <div class="btn" @click="append('7')">7</div>
    <div class="btn" @click="append('8')">8</div>
    <div class="btn" @click="append('9')">9</div>
    <div class="btn operator" @click="multiply">x</div>
    <div class="btn" @click="append('4')">4</div>
    <div class="btn" @click="append('5')">5</div>
    <div class="btn" @click="append('6')">6</div>
    <div class="btn operator" @click="subtract">-</div>
    <div class="btn" @click="append('1')">1</div>
    <div class="btn" @click="append('2')">2</div>
    <div class="btn" @click="append('3')">3</div>
    <div class="btn operator" @click="add">+</div>
    <div class="btn col-span-2 rounded-bl-lg" @click="append('0')">0</div>
    <div class="btn" @click="dot">.</div>
    <div class="btn operator rounded-br-lg" @click="equal">=</div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'

  const previous = ref(null)
  const current = ref('')
  const operator = ref(null)
  const operatorClicked = ref(false)

  const clear = () => { current.value = '' }
  const sign = () => { current.value = current.value.charAt(0) === '-' ? current.value.slice(1) : `-${current.value}` }
  const percent = () => { current.value = `${parseFloat(current.value) / 100 }` }

  const append = (number) => {
    if (operatorClicked.value) {
      current.value = ''
      operatorClicked.value = false
    }
    current.value = `${current.value}${number}`
  }
  const dot = () => {
    if (current.value.indexOf('.') === -1) {
      append('.')
    }
  }

  const setPrevious = () => {
    previous.value = current.value
    operatorClicked.value = true
  }

  const divide = () => {
    operator.value = (a, b) => a / b
    setPrevious()
  }
  const multiply = () => {
    operator.value = (a, b) => a * b
    setPrevious()
  }
  const subtract = () => {
    operator.value = (a, b) => b - a
    setPrevious()
  }
  const add = () => {
    operator.value = (a, b) => a + b
    setPrevious()
  }

  const equal = () => { current.value = `${operator.value(
      parseFloat(previous.value),
      parseFloat(current.value)
    )}`
    previous.value = null
  }

</script>

<style scoped>
  .calculator {
    @apply max-w-[15rem] grid grid-cols-4 auto-rows-[minmax(50px,_auto)] justify-center mt-1 mx-auto text-center
  }

  .calcField {
    @apply bg-gray-800 text-white col-span-full text-3xl font-extralight flex items-center justify-center shadow-inner cursor-text;
  }
  .btn {
    @apply bg-[#5c5c5c] text-white border-[.5px] border-[#272727]/80 align-middle text-2xl font-extralight flex items-center justify-center cursor-pointer hover:bg-gray-300;

    &.operator {
      @apply !bg-amber-500 hover:!bg-amber-600;
    }
  }

</style>