<template lang="pug">
  v-container
    v-row(justify="center")
      v-col.offset-0(cols="12" md="4").calc
        v-row
          v-col(cols = "12")
            div.input-display.text-right {{valueDisplayed}}
          v-col(cols = "12")
            v-btn.error(block @click='reset()') C
          v-col(cols = '3' v-for='operation in operations')
            v-btn.primary(@click='handleDigit(operation.text)' block
              v-if="operation.type === 'number'" )
              | {{operation.text}}
            v-btn.primary(@click='handleOp(operation.type)' block v-else)
              | {{operation.text}}
</template>

<script>
export default {
  name: 'Calc',
  data: () => ({
    currentValue: 0,
    savedValue: false,
    currentOp: false,
    operations: [
      { text: '1', type: 'number' },
      { text: '2', type: 'number' },
      { text: '3', type: 'number' },
      { text: '+', type: 'sum' },
      { text: '4', type: 'number' },
      { text: '5', type: 'number' },
      { text: '6', type: 'number' },
      { text: '-', type: 'difference' },
      { text: '7', type: 'number' },
      { text: '8', type: 'number' },
      { text: '9', type: 'number' },
      { text: 'x', type: 'multiply' },
      { text: '=', type: 'equal' },
      { text: '0', type: 'number' },
      { text: '.', type: 'number' },
      { text: '/', type: 'division' },
    ],
  }),
  methods: {
    reset() {
      this.currentValue = 0;
      this.savedValue = false;
      this.currentOp = false;
    },
    handleDigit(digit) {
      if (this.currentOp === 'equal') {
        this.savedValue = false;
      }
      this.currentValue = (this.currentValue) ? this.currentValue + digit : digit;
    },
    handleOp(op) {
      if (this.currentOp) {
        this.process();
      } else {
        this.savedValue = parseFloat(this.currentValue);
      }
      this.currentValue = 0;
      this.currentOp = op;
    },
    process() {
      const currentValueNumber = parseFloat(this.currentValue);
      switch (this.currentOp) {
        case 'sum':
          this.savedValue += currentValueNumber;
          break;
        case 'difference':
          this.savedValue -= currentValueNumber;
          break;
        case 'multiply':
          this.savedValue *= currentValueNumber;
          break;
        case 'division':
          this.savedValue /= currentValueNumber;
          break;
        default:
          this.savedValue = currentValueNumber;
          break;
      }
      this.currentValue = 0;
      this.currentOp = false;
    },
  },
  computed: {
    valueDisplayed() {
      return (!this.savedValue || this.currentValue) ? this.currentValue : this.savedValue;
    },
  },
};
</script>

<style lang="sass">
@import url('https://fonts.googleapis.com/css2?family=Oxanium:wght@500&display=swap')
.calc
  font-family: 'Oxanium', monospace
.input-display
  font-size: xx-large
  background: mediumseagreen
  color: black
  box-shadow: inset 0 0 10px rgba(0,0,0,0.9)
  padding: 12px
  user-select: none
</style>
