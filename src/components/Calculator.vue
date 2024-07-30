<script setup>
import { ref } from 'vue';
const calculatorValue = ref('');
const operator = ref(null);
const previousCalculatorValue = ref('');
const calculatorElements = ['AC','+/-','%','/',7,8,9,'*',4,5,6,'-',1,2,3,'+',0,'.','='];

function action(n) {
  if (!isNaN(n) || n === '.') {
    if (this.operator != null) {
      this.calculatorValue = n + '';
    }
    else {
      this.calculatorValue += n + '';
    }
  }
  if (n === 'AC') {
    this.calculatorValue = '';
  }
  if (n === '%') {
    this.calculatorValue = this.calculatorValue / 100 + '';
  }
  if (['/','*','+','-'].includes(n)) {
    this.operator = n;
    this.previousCalculatorValue = this.calculatorValue;
  }
  if (n === '=') {
    this.calculatorValue = eval(
      this.previousCalculatorValue + this.operator + this.calculatorValue
    );
    this.previousCalculatorValue = '';
    this.operator = null;
  }
  if (n === '+/-') {
    if (this.calculatorValue > 0) {
      this.calculatorValue = '-' + this.calculatorValue
    }
  }
}

</script>

<template>
  <div class="calculator p-3" style="max-width: 400px; margin: 50px auto; background: #234;">
    <div class="w-full rounded m-1 p-3 lead text-white bg-vue-dark result">
      {{ calculatorValue }}
    </div>
    <div class="row no-gutters">
      <div class="col-3" v-for="n in calculatorElements" :key="n">
        <div class="lead text-white text-center m-1 py-3 bg-vue-dark rounded hover-class"
          :class="{'bg-vue-green' : ['/','*','+','=','-'].includes(n)}"
          @click="action(n)"
        >
          {{ n }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .bg-vue-dark {
    background:  #31475e;
  }
  .result {
    text-align: right;
    font-weight: bold;
    min-height: 65px;
  }
  .hover-class:hover{
    cursor: pointer;
    background: #3D5875;
  }
  .bg-vue-green {
    background: #3fb984;
  }
  .bg-vue-grey {
    background: #616870;
  }
</style>
