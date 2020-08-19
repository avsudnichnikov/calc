<template lang="pug">
  v-container
    v-row(justify="center")
      v-col.offset-0(cols="12" md="4")
        v-row
          v-col(cols = "12")
            div.input-display.text-right {{valueDisplayed}}
          v-col(cols = "12")
            v-btn.error(block @click='reset()') С
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
      if (this.currentOp === 'sum') {
        this.savedValue += currentValueNumber;
      } else if (this.currentOp === 'difference') {
        this.savedValue -= currentValueNumber;
      } else if (this.currentOp === 'multiply') {
        this.savedValue *= currentValueNumber;
      } else if (this.currentOp === 'division') {
        this.savedValue /= currentValueNumber;
      } else if (this.currentOp === 'equal' && currentValueNumber) {
        this.savedValue = currentValueNumber;
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
.input-display
  font-size: xx-large
  background: mediumseagreen
  color: black
  padding: 12px
</style>
