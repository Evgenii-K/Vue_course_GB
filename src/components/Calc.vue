<template>
  <div class="content">
    <div class="wrapper">
      <div class="display">{{ display }}</div>
      <div class="grid__wrapper">
        <button @click="clear" class="btn">{{ clearBtn }}</button>
        <button @click="del" class="btn">←</button>
        <button @click="percent" class="btn">%</button>
        <button @click="calc" v-bind:class="{active: currentOperetion === '/' && inputNewNumber}" class="btn btn__calc">/</button>
        <button @click="displayNumber" class="btn btn__number">7</button>
        <button @click="displayNumber" class="btn btn__number">8</button>
        <button @click="displayNumber" class="btn btn__number">9</button>
        <button @click="calc" v-bind:class="{active: currentOperetion === '*' && inputNewNumber}" class="btn btn__calc">*</button>
        <button @click="displayNumber" class="btn btn__number">4</button>
        <button @click="displayNumber" class="btn btn__number">5</button>
        <button @click="displayNumber" class="btn btn__number">6</button>
        <button @click="calc" v-bind:class="{active: currentOperetion === '-' && inputNewNumber}" class="btn btn__calc">-</button>
        <button @click="displayNumber" class="btn btn__number">1</button>
        <button @click="displayNumber" class="btn btn__number">2</button>
        <button @click="displayNumber" class="btn btn__number">3</button>
        <button @click="calc" class="btn btn__calc" v-bind:class="{active: currentOperetion === '+' && inputNewNumber}">+</button>
        <button @click="displayNumber" class="btn btn__number btn__null">0</button>
        <button @click="dot" class="btn btn__number">.</button>
        <button @click="calc" class="btn btn__number">=</button>
      </div>
    </div>
   </div>
</template>

<script>
export default {
  name: 'Calc',
  data () {
    return {
      display: 0,
      inputNewNumber: false,
      currentTotal: 0,
      currentOperetion: '',
      clearBtn: 'AC'
    }
  },
  methods: {
    onKeyDown (e) {
      if ((e.key).match(/\./)) {
        this.dot()
      } else if ((e.key).match(/[0-9]/)) {
        this.displayNumber(e)
      } else if ((e.key).match(/%/)) {
        this.percent()
      } else if ((e.key).match(/Enter/)) {
        this.calc(e)
      } else if ((e.key).match(/[-+*/]/)) {
        this.calc(e)
      } else if ((e.key).match(/Backspace/)) {
        this.del()
      }
    },
    calc (e) {
      let inputOperetion = ''
      if (e.type === 'keydown') {
        if (e.key === 'Enter') {
          inputOperetion = '='
        } else {
          inputOperetion = e.key
        }
      } else {
        inputOperetion = e.target.textContent
      }
      const op = this.currentOperetion
      const inputNumber = +this.display

      if (this.inputNewNumber && op !== '=') {
        this.display = this.currentTotal
      } else {
        this.inputNewNumber = true
        switch (op) {
          case '+':
            this.currentTotal += inputNumber
            break
          case '-':
            this.currentTotal -= inputNumber
            break
          case '*':
            if (this.currentTotal) {
              this.currentTotal *= inputNumber
            } else {
              this.currentTotal = inputNumber
            }
            break
          case '/':
            if (this.currentTotal) {
              this.currentTotal /= inputNumber
            } else {
              this.currentTotal = inputNumber
            }
            break
          default:
            this.currentTotal = inputNumber
        }
      }

      this.display = this.currentTotal

      if (this.currentTotal === Infinity) {
        this.inputNewNumber = false
        this.currentTotal = 0
        this.currentOperetion = ''
      } else {
        this.currentOperetion = inputOperetion
      }
    },
    percent () {
      const op = this.currentOperetion
      let inputNumber = +this.display
      if (op === '') {
        inputNumber = inputNumber / 100
      } else {
        inputNumber = inputNumber / 100 * this.currentTotal
      }

      this.display = inputNumber
    },
    displayNumber (e) {
      if (this.display.length > 9) {
        return
      }

      let inputNumber = ''
      if (e.type === 'keydown') {
        inputNumber = e.key
      } else {
        inputNumber = e.target.textContent
      }

      if (this.inputNewNumber) {
        this.display = inputNumber
        this.inputNewNumber = false
      } else {
        if (+this.display === 0 && !this.display.toString().includes('.')) {
          this.display = inputNumber
        } else {
          this.display += inputNumber
        }
      }
    },
    clear () {
      if (this.clearBtn === 'AC') {
        this.display = 0
        this.inputNewNumber = false
        this.currentTotal = 0
        this.currentOperetion = ''
      } else {
        this.display = 0
      }
    },
    dot () {
      if (this.display.length > 9) {
        return
      }

      if (this.inputNewNumber) {
        this.display = '0.'
        this.inputNewNumber = false
      } else {
        if (!this.display.toString().includes('.')) {
          this.display += '.'
        }
      }
    },
    del () {
      if (this.display.length > 1) {
        this.display = this.display.slice(0, -1)
      } else {
        this.display = 0
      }
    }
  },
  watch: {
    display () {
      if (this.display) {
        this.clearBtn = 'C'
      } else {
        this.clearBtn = 'AC'
      }
    }
  },
  created () {
    document.addEventListener('keydown', this.onKeyDown)
  },
  beforeDestroy () {
    document.addEventListener('keydown', this.onKeyDown)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
* {
    margin: 0;
    padding: 0;
    border: 0;
}

button::-moz-focus-inner {
    padding: 0;
    border: 0;
}

.content {
  display: flex;
  justify-content: center;
  padding-top: 30px;
}

.wrapper {
  display: flex;
  flex-direction: column;
  width: 325px;
  align-items: flex-end;
}

.display {
  font-weight: 400;
  font-size: 58px;
  -ms-text-size-adjust: 100%;
  -moz-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;
  padding-bottom: 20px;
  max-width: 325px;
}

.grid__wrapper {
  display: flex;
  justify-content: space-between;
  align-items: space-between;
  flex-wrap: wrap;
  height: 410px;
}

.btn {

  font-weight: 600;
  font-size: 18px;
  -ms-text-size-adjust: 100%;
  -moz-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -webkit-font-smoothing: antialiased;

  width: 70px;
  height: 70px;

  border-radius: 50%;

  background-color:lightgray;
  transition: background-color 0.15s linear;

  &:hover {
    background-color: whitesmoke;
    transition: background-color 0.15s linear;
  }

  &__number {
    background-color: grey;

    &:hover {
      background-color: lightgrey;
    }
  }

  &__calc {
    background-color: orange;
    font-weight: 600;
    font-size: 22px;

    &:hover {
      background-color: rgb(245, 187, 79);
    }
  }

  &__null {
    width: 155px;
    border-radius: 35px;
  }
}

.active {
  background-color: rgb(255, 78, 8);
  color: whitesmoke;
}
</style>
