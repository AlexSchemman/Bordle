<template>
  <v-container>
    {{ clickedLetters }}
    <div class="keychoice">
      <div class="row">
        <div 
          v-for="letter in letters.slice(0,10)" 
          :key="letter"
          @mouseenter="HoverEl = letter"
          @mouseleave="HoverEl = HoverEl==letter? 'null' : letter"
          >
          <v-btn
            :class="{ floatLastClicked: letter == lastClicked, floatClicked: clickedLetters.includes(letter), floatPrev: letter == prevL, floatNext: letter == nextL, floatMain: letter == HoverEl}"
            @click="addLetter(letter)"
          >

          </v-btn>
        </div>
      </div>
      <div class="row">
        <div
          v-for="letter in letters.slice(10,19)" 
          :key="letter"
          @mouseenter="HoverEl = letter"
          @mouseleave="HoverEl = HoverEl==letter? 'null' : letter"
          >
          <v-btn
            :class="{floatLastClicked: letter == lastClicked, floatClicked: clickedLetters.includes(letter), floatPrev: letter == prevL, floatNext: letter == nextL, floatMain: letter == HoverEl}"
            @click="addLetter(letter)"
          >

          </v-btn>
        </div>
      </div>
      <div class="row">
        <div 
          v-for="letter in letters.slice(19,26)" 
          :key="letter"
          @mouseenter="HoverEl = letter"
          @mouseleave="HoverEl = HoverEl==letter? 'null' : letter"
          >
          <v-btn
            :class="{ floatLastClicked: letter == lastClicked, floatClicked: clickedLetters.includes(letter), floatPrev: letter == prevL, floatNext: letter == nextL, floatMain: letter == HoverEl}"
            @click="addLetter(letter)"
          >

          </v-btn>
        </div>
      </div>
    </div>
    {{ prevL }}
    {{ HoverEl }}
    {{ nextL }}
  </v-container>
</template>

<script>
import axios from 'axios'

export default {
  name: 'HelloWorld',
  props: {
    Chosen: {
      type: String,
      required: true,
      default: '',
    },
    settingsConfig: {
      type: Object,
      required: true,
      default: () => {},
    },
  },
  methods: {
    addLetter (letter) {
      this.clickedLetters.push(letter);
      this.lastClicked = letter;
    },
    undoAddLetter () {
      if(this.clickedLetters.length > 0){
        this.clickedLetters.pop();
        this.lastClicked = this.clickedLetters.slice(-1);
      }
    },
    shuffleRows () {
      let currentIndex = this.letters.length,  randomIndex;
      while (currentIndex != 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [this.letters[currentIndex], this.letters[randomIndex]] = [
          this.letters[randomIndex], this.letters[currentIndex]];
      }
    },
    keyupHandler (event) {
      if (event.ctrlKey && event.code === 'KeyZ') {
        this.undoAddLetter();
      }
    },
    async getWordle() {
      const response = await axios.post('https://rapidapi.com/learn/api/rest', {
        guess: 'John',
        });
      console.log(response)
    }
  },
  created() {
    this.getWordle();
    document.addEventListener('keyup', this.keyupHandler)

    const IntList = Array.from(Array(26)).map((e, i) => i + 65);
    this.letters = IntList.map((x) => String.fromCharCode(x));

    this.shuffleRows();
  },
  unmounted () {
    document.removeEventListener('keyup', this.keyupHandler)
  },
  watch: {
    HoverEl(newVal){
      const code = newVal.charCodeAt(0);
      this.prevL = String.fromCharCode(code-1);
      this.nextL = String.fromCharCode(code+1);
    }
  },
  data() {
    return {
      letters: [],
      clickedLetters: [],
      lastClicked: '',
      HoverEl: '',
      nextL: '',
      prevL: '',
    }
  }
}
</script>

<style>

</style>

<style scoped lang="scss">
.v-container {
  
}

.v-btn {
  font-size: calc((var(--baseSize)/4)*1);
  margin: calc((var(--baseSize))/9);
  height: calc((var(--baseSize))*1.4) !important;
  width: calc((var(--baseSize))*1);
  min-width: var(--baseSize);
  min-height: var(--baseSize) !important;
  border-radius: 0.5em;

  // 1em 1em 0em 0em

  align-items: center;
  display: inline-grid;
  justify-content: center;
  letter-spacing: 0.0892857143em;
  text-indent: 0.0892857143em;
  line-height: normal;
  text-transform: uppercase;
  flex-shrink: 0;
  box-shadow: none;
  transition: all 0.6s ease;
  color: #EBF2FA;
  background-color: #427AA1;
}

.floatMain {
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3);
  -webkit-transform: scale(1.1, 1.1);
  background-color: #05668d;
  transform: scale(1.1, 1.1);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out ;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out;
  border-radius: 0em;
}

.floatPrev {
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3);
  background-color: #05668d;
  -webkit-transform: scale(1.05, 1.05);
  transform: scale(1.05, 1.05);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out ;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out;
  border-radius: 0em;
}

.floatNext {
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3);
  background-color: #05668d;
  -webkit-transform: scale(1.05, 1.05);
  transform: scale(1.05, 1.05);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out ;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out;
  border-radius: 0em;
}

.floatClicked {
  box-shadow: 0px 3px 1px -2px var(--v-shadow-key-umbra-opacity, 
  rgba(0, 0, 0, 0.2)), 0px 2px 2px 0px var(--v-shadow-key-penumbra-opacity,
    rgba(0, 0, 0, 0.14)), 0px 1px 5px 0px var(--v-shadow-key-penumbra-opacity,
    rgba(0, 0, 0, 0.12));
  background-color: red !important;
}

.floatLastClicked {
  box-shadow: 0px 3px 1px -2px var(--v-shadow-key-umbra-opacity, 
    rgba(0, 0, 0, 0.2)), 0px 2px 2px 0px var(--v-shadow-key-penumbra-opacity,
     rgba(0, 0, 0, 0.14)), 0px 1px 5px 0px var(--v-shadow-key-penumbra-opacity,
      rgba(0, 0, 0, 0.12));
  background-color: yellow !important;
}

.none {
  background-color: red;
}
.row {
  align-self: center;
  display: inline-flex;
}

.keychoice {
  display: flex;
  flex-direction: column;
}
</style>

