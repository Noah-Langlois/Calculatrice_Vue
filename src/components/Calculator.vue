<script setup>
import { ref } from 'vue';
const calculatorValue = ref('');
const operator = ref(null);
const previousCalculatorValue = ref('');
const calculatorElements = ['AC','+/-','%','/',7,8,9,'*',4,5,6,'-',1,2,3,'+',0,'.','='];
const history_results = ref([])

function action(n) {
  if (!isNaN(n) || n === '.') {
    if (this.operator != null && this.calculatorValue != '-') {
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
    if (this.calculatorValue != '' && this.previousCalculatorValue == '') {
      this.operator = n;
      this.previousCalculatorValue = this.calculatorValue;
    }
    else if (n === '-') {
      this.calculatorValue = n + '';
    }
  }
  if (n === '=') {
    this.calculatorValue = eval(
      this.previousCalculatorValue + this.operator + this.calculatorValue
    );
    this.history_results.unshift(this.calculatorValue)
    this.previousCalculatorValue = '';
    this.operator = null;
  }
  if (n === '+/-') {
    if (this.calculatorValue > 0) {
      this.calculatorValue = '-' + this.calculatorValue
    }
    else {
      this.calculatorValue = this.calculatorValue*-1
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
      <div class="col-3" v-for="n in calculatorElements" :key="n"
        :class="{'zero' : n === 0}"
      >
        <div class="lead text-white text-center m-1 py-3 bg-vue-dark rounded hover-class"
          :class="{'bg-vue-orange' : ['/','*','+','=','-'].includes(n),
            'bg-vue-grey' : ['AC','+/-','%'].includes(n)
          }"
          @click="action(n)"
        >
          {{ n }}
        </div>
      </div>
    </div>
  </div>
  <div class="history text-center text-white">
    <div class="result_element" v-for="n in history_results" :key="n">
      {{ n }}
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
  .bg-vue-orange {
    background: #bd8611;
  }
  .bg-vue-grey {
    background: #8f9ba8;
  }
  .zero {
    width: 50%;
  }
</style>
