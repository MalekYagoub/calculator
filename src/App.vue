<template>
  <v-app>
    <v-content>
      <v-container class="main-container">
        <v-row justify="center" class="mb-3">
          <Screen
            ref="screen"
            :op="op"
            :correctAnswers="correctAnswers"
            :incorrectAnswers="incorrectAnswers"
          />
        </v-row>
        <v-row justify="center" class="mb-5">
          <v-col v-for="calculationType in calculationsTypes" :key="calculationType" cols="auto">
            <CalculationType
              :calculationType="calculationType"
              :selectedType="selectedType"
              @emit-change-selected-type="changeSelectedType"
            />
          </v-col>
        </v-row>
        <v-row justify="center" dense>
          <v-col v-for="number in 100" :key="number" cols="auto">
            <NumberChoice
              ref="numbers"
              @emit-number-click="checkUserResult"
              :number="number - 1"
              :incorrectUserResults="incorrectUserResults"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>

import Screen from '@/components/Screen';
import CalculationType from '@/components/CalculationType';
import NumberChoice from '@/components/NumberChoice';

export default {
  name: 'App',
  components: {
    Screen,
    CalculationType,
    NumberChoice
  },
  data: () => ({
    selectedType: '×',
    calculationsTypes: ['×', '+', '-', '?'],
    op: null,
    correctAnswers: 0,
    incorrectAnswers: 0,
    incorrectUserResults: []
  }),
  methods: {
    getCalculation (type) {
      this.op = {};
      this.incorrectUserResults = [];

      switch(type) {
        case '×' :
          this.op.digit1 = Math.floor(0.6 * (3 + Math.random() * 7));
          this.op.digit2 = Math.floor(1 + Math.random() * 10 );
          this.op.opChar = type;
          this.op.result = this.op.digit1 * this.op.digit2;
          break;

        case '+' :
          this.op.digit1 = Math.floor(Math.random() * 99 );
          this.op.digit2 = Math.floor(1 + Math.random() * (99 - this.op.digit1));
          this.op.digit1 = Math.floor(this.op.digit1 * 0.6);
          this.op.digit2 = Math.floor(this.op.digit2 * 0.6);
          this.op.opChar = type;
          this.op.result = this.op.digit1 + this.op.digit2;
          break;

        case '-' :
          this.op.digit1 = Math.floor( 10 + Math.random() * 90);
          this.op.digit2 = Math.floor( Math.random() * this.op.digit1);
          this.op.digit1 = Math.floor(this.op.digit1 * 0.6);
          this.op.digit2 = Math.floor(this.op.digit2 * 0.6);
          this.op.opChar = type;
          this.op.result = this.op.digit1 - this.op.digit2;
          break;

        case '?':
          this.getCalculation(this.calculationsTypes[Math.floor(Math.random() * 3)]);
          break;
      }
    },
    checkUserResult (userResult) {
      let stateClass = '';
      if (userResult == this.op.result) {
        stateClass = 'correct-number';
        this.correctAnswers++;
      } else {
        stateClass = 'incorrect-number';
        this.incorrectAnswers++;
      }

      this.$refs.numbers[userResult].$el.classList.add(stateClass);
      this.$refs.screen.$el.classList.add(stateClass);
      setTimeout(() => {
        this.$refs.numbers[userResult].$el.classList.remove(stateClass);
        this.$refs.screen.$el.classList.remove(stateClass);
        if (stateClass === 'incorrect-number') {
          this.incorrectUserResults.push(userResult);
        } else {
          this.getCalculation(this.selectedType);
        }
      }, 250);
    },
    changeSelectedType (calculationType) {
      this.selectedType = calculationType;
      this.getCalculation(calculationType);
    }
  },
  mounted () {
    this.getCalculation(this.selectedType);
  }
};
</script>

<style>
  main, body {
    background-color: #e8e8e8;
  }

  .main-container {
    width: 800px !important;
  }
</style>
