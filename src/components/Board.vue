<template>
  <v-container>
    Reference alphabet : {{ AlphabetString }}
    <div class="keychoice">
      <div class="row">
        <div 
          v-for="letter in letters.slice(0,10)" 
          :key="letter"
          @mouseenter="HoverEl = letter"
          @mouseleave="HoverEl = HoverEl==letter? 'null' : letter"
          >
          <v-btn
            :class="{floatLastClicked: letter == lastClicked, floatClicked: clickedLetters.includes(letter), floatPrev: letter == prevL, floatNext: letter == nextL, floatMain: letter == HoverEl}"
            @click="addLetter(letter)"
          >
          <div v-if="renderLetter(letter)">
            {{ letter }}
          </div>
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
            <div 
            v-if="renderLetter(letter)">
            {{ letter }}
          </div>
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
            :class="{floatLastClicked: letter == lastClicked, floatClicked: clickedLetters.includes(letter), floatPrev: letter == prevL, floatNext: letter == nextL, floatMain: letter == HoverEl}"
            @click="addLetter(letter)"
          >
            <div v-if="renderLetter(letter)">
            {{ letter }}
          </div>
          </v-btn>
        </div>
      </div>
    </div>
    <!-- {{ prevL }}
    {{ HoverEl }}
    {{ nextL }} -->
  </v-container>
</template>

<script>
export default {
  name: 'boardSection',
  props: {
    chosenWord: {
      type: String,
      required: false,
      default: '',
    }
  },
  methods: {
    renderLetter(letter) {
      return (
        this.chosenWord.includes(String.fromCharCode(letter.charCodeAt(0)-1)) && 
        this.clickedLetters.includes(String.fromCharCode(letter.charCodeAt(0)-1))
      ) || (
        this.clickedLetters.includes(String.fromCharCode(letter.charCodeAt(0)+1)) &&
        this.chosenWord.includes(String.fromCharCode(letter.charCodeAt(0)+1))
      ) || (
        this.clickedLetters.includes(letter)
      )
    },
    addLetter (letter) {
      if(!this.clickedLetters.includes(letter)){
        this.lastClicked = letter;
        this.clickedLetters.push(letter);
        this.$emit('clicked-letters', this.clickedLetters)
      }
    },
    undoAddLetter () {
      if(this.clickedLetters.length > 0){
        this.clickedLetters.pop();
        this.$emit('clicked-letters', this.clickedLetters)
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
      else if (event.keyCode > 64 && event.keyCode < 91) 
      {
        this.setPressedKey(event.code);
      }
    },
    setPressedKey(key) {
      key = key.replace("Key","")
      var index = this.keeb.indexOf(key);

      if(this.letters[index] == this.pressedKey) {
        this.addLetter(this.letters[index]);
        this.HoverEl = '';
      }
      else {
        this.HoverEl = this.letters[index];
        this.pressedKey = this.letters[index];
      }
    },
    resetGame() {
      this.clickedLetters = [];
      this.pressedKey = '';
      this.lastClicked = '';
      this.HoverEl = '';
      
      this.shuffleRows();
      this.clickedLetters.push(this.chosenWord[0])
      this.$emit('clicked-letters', this.clickedLetters)
    }
  },
  created() {
    document.addEventListener('keyup', this.keyupHandler)

    let codeArr = Array.from(Array(26)).map((e, i) => i + 65);
    let intList = codeArr.map((x) => String.fromCharCode(x));
    this.letters = [...intList];
    this.AlphabetString = intList.join(" ")
    this.shuffleRows();
    this.clickedLetters.push(this.chosenWord[0])
    this.$emit('clicked-letters', this.clickedLetters)
  },
  unmounted () {
    document.removeEventListener('keyup', this.keyupHandler)
  },
  watch: {
    HoverEl(newVal){
      const code = newVal.charCodeAt(0);
      this.prevL = String.fromCharCode(code-1);
      this.nextL = String.fromCharCode(code+1);
    },
  },
  data() {
    return {
      letters: [],
      keeb: [
        'Q', 'W', 'E', 'R', 'T', 'Y', 'U', 'I', 'O', 'P', 
        'A', 'S', 'D', 'F', 'G', 'H', 'J', 'K', 'L',
        'Z', 'X', 'C', 'V', 'B', 'N', 'M'
      ],
      AlphabetString: '',
      clickedLetters: [],
      pressedKey: '',
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
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3) !important;
  -webkit-transform: scale(1.1, 1.1);
  background-color: #05668d;
  transform: scale(1.1, 1.1);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  border-radius: 0em;
}

.floatPrev {
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3) !important;
  background-color: #05668d;
  -webkit-transform: scale(1.05, 1.05);
  transform: scale(1.05, 1.05);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  border-radius: 0em;
}

.floatNext {
  box-shadow: 0 0px 25px rgba(0, 0, 0, 0.3) !important;
  background-color: #05668d;
  -webkit-transform: scale(1.05, 1.05);
  transform: scale(1.05, 1.05);
  -webkit-transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  transition: transform 0.3s ease-in-out, border-radius 0.3s ease-in-out, background-color 0.3s ease-in-out;
  border-radius: 0em;
}

.floatClicked {
  box-shadow: 0px 3px 1px -2px var(--v-shadow-key-umbra-opacity, 
  rgba(0, 0, 0, 0.2)), 0px 2px 2px 0px var(--v-shadow-key-penumbra-opacity,
    rgba(0, 0, 0, 0.14)), 0px 1px 5px 0px var(--v-shadow-key-penumbra-opacity,
    rgba(0, 0, 0, 0.12));
  background-color: red !important;
  color: yellow;
}

.floatLastClicked {
  box-shadow: 0px 3px 1px -2px var(--v-shadow-key-umbra-opacity, 
    rgba(0, 0, 0, 0.2)), 0px 2px 2px 0px var(--v-shadow-key-penumbra-opacity,
     rgba(0, 0, 0, 0.14)), 0px 1px 5px 0px var(--v-shadow-key-penumbra-opacity,
      rgba(0, 0, 0, 0.12));
  background-color: yellow !important;
  color: red;
}

.floatCorrectClicked {
  color: #EBF2FA;
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

