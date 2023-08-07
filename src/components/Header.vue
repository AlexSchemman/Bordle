<template>
  <v-container>
    <div class="row">
      <div
      class="wordBox"
      v-for="letter in word"
      :key="letter"
      >
        <div 
        class="word"
        :class="{ clicked: clickedLetterArray.includes(letter)}"
        >
          {{ letter }}
        </div>
      </div>
      <Transition>
        <div 
        class="reset-btn"
        v-if="showReset"
        >
          <v-icon icon="mdi-restart"></v-icon>
        </div>
      </Transition>
    </div>
    <div>
      <v-btn 
      
        >
        Reset
      </v-btn>
    </div>
  </v-container>
</template>

<script>
import TextFile from "raw-loader!../assets/google-10000-english-no-swears.txt"
export default {
  name: 'headerSection',
  props: {
    clickedLetters: {
      type: String,
      required: false,
      default: '',
    }
  },
  methods: {
    async getWordle() {
      let wordArr = TextFile.split(/[\n\r]+/).filter(words => words.length === 5)
      this.word = wordArr[wordArr.length * Math.random() | 0].toUpperCase().split('');
      console.log(this.word)
      this.$emit('chosen-word', this.word)
    },
    resetGame() {
      this.$emit('reset');
      this.getWordle();
    }
  },
  watch: {
    clickedLetters(newVal) {
      this.clickedLetterArray = newVal.split("");
      if(this.word.every(v => this.clickedLetters.includes(v)) && this.showReset == false){
        this.score = 26 - this.clickedLetterArray.length;
        this.showReset = true;
        console.log('reset')
        
        setTimeout(() => {
          alert("Score is " + this.score)
        }, 1000)
      }
    }
  },
  data() {
    return {
      word: '',
      clickedLetterArray : [],
      score: '',
      showReset: false,
    }
  },
  created() {
    this.getWordle();
  },
}
</script>


<!-- Add "scoped" attribute to limit CSS to this component only -->

<style scoped lang="scss">

.v-container {

}

.v-enter-active,
.v-leave-active {
  transition: opacity 0.3s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}

.word {
  font-size: calc((var(--baseSize)/4)*1);
  margin: calc((var(--baseSize))/9);
  height: calc((var(--baseSize))*1) !important;
  width: calc((var(--baseSize))*1);
  min-width: var(--baseSize);
  min-height: var(--baseSize) !important;
  border-radius: 0.5em;

  align-items: center;
  display: inline-grid;
  justify-content: center;
  letter-spacing: 0.0892857143em;
  text-indent: 0.0892857143em;
  line-height: normal;
  text-transform: uppercase;
  flex-shrink: 0;
  box-shadow: inset 0 0px 6px 2px rgba(0, 0, 0, 0.3);
  color: #ebf2fa;
}

.reset-btn {
  position: absolute;
  right: -2.75em;
  margin: calc((var(--baseSize))/9);
  height: calc((var(--baseSize))*0.5) !important;
  width: calc((var(--baseSize))*0.5);
  border-radius: 0.5em;

  align-items: center;
  display: inline-grid;
  justify-content: center;

  box-shadow: 0px 3px 1px -2px var(--v-shadow-key-umbra-opacity, 
  rgba(0, 0, 0, 0.2)), 0px 2px 2px 0px var(--v-shadow-key-penumbra-opacity,
   rgba(0, 0, 0, 0.14)), 0px 1px 5px 0px var(--v-shadow-key-penumbra-opacity,
    rgba(0, 0, 0, 0.12));
  background-color: rebeccapurple;
  color: #ebf2fa;
}

.clicked {
  color: #427AA1;
}

.row {
  align-self: center;
  display: inline-flex;
  position: relative;
}
</style>
