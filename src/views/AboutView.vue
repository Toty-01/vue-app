<template>
  <div class="about">
    <h1>Calculatrice</h1>

    <div class="calc">
    <div class="p-3">

      <div class="w-full rounded m-1 p-3 text-right lead font-weight-bold text-white bg-vue-dark">
        {{ calculatorValue || 0 }}
      </div>

        <div class="row no-gutters">
          <div class="col-3" v-for="n in calculatorElements" :key="n">

            <div class="lead text-white text-center mt-2 py-3 bg-vue-dark rounded hover-class"
              :class="{'bg-vue-green': ['C','*','/','-','+','%','='].includes(n)}"
            @click="action(n)">
              {{n}}
            </div>

          </div>
        </div>
    </div>
  </div>
  </div>
</template>

<script>
import { thisExpression } from '@babel/types';

export default {
  name: 'AboutView',
  data() {
    return {
      calculatorValue: '',
      calculatorElements:['C','*','/','-',7,8,9,'+',4,5,6,'%',1,2,3,'=',0,'.'],
      operator: null,
      previousCalculatorValue: '',
    }
  },
  methods: {
    action(n){
      if(!isNaN(n) || n === '.'){
        this.calculatorValue += n + '';
      }
      /*Clear Value*/
      if(n === 'C'){
        this.calculatorValue = '';
      }
      /*Modulo Value*/
      if(n === '%'){
        this.calculatorValue = this.calculatorValue / 100 + '';
      }
      if(['/','*','-','+'].includes(n)){
        this.operator = n;
        this.previousCalculatorValue = this.calculatorValue;
        this.calculatorValue = '';
      }
      /*Egal Value*/
      if(n === '='){
        this.calculatorValue = eval(
          this.previousCalculatorValue + this.operator + this.calculatorValue
        );
        this.previousCalculatorValue = '',
        this.operator = null;
      }
    }
  }
}
</script>

<style scoped>
  .p-3 {
    max-width: 400px;
    margin: 50px auto;
    background-color: #234;
  }
  .w-full {
    text-align: end;
  }
  .bg-vue-dark {
   background: #31475e;
  }
  .hover-class:hover {
    cursor: pointer;
    background-color: #3D5875;
  }
  .bg-vue-green {
    background: #3fb984;
  }
</style>