<template>
  <div class="calculator">
    <h1>Calculadora Aritmética</h1>

    <div class="inputs">
      <input v-model.number="num1" type="number">
      <select v-model="operation">
        <option value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
      </select>
      <input v-model.number="num2" type="number">
    </div>

    <div class="result">
      <h2>Resultado: {{ calculate }}</h2>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      num1: 0,
      num2: 0,
      operation: '+',
    }
  },
  computed: {
    calculate() {
      switch (this.operation) {
        case '+':
          return this.num1 + this.num2
        case '-':
          return this.num1 - this.num2
        case '*':
          return this.num1 * this.num2
        case '/':
          return this.num1 / this.num2
        default:
          return 0
      }
    },
  },
}
</script>

<style scoped>
.calculator {
  max-width: 400px;
  margin: 0 auto;
  text-align: center;
}

.inputs {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}

input,
select {
  font-size: 18px;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin: 0 10px;
  width: 100px;
}

.result {
  border: 1px solid #ccc;
  padding: 20px;
  border-radius: 4px;
}
</style>
