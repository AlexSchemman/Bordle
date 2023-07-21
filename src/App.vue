<template>
  <v-app>
    <Header
    @chosen-word="onChosenWord"
    @reset="$refs.resetTrigger.resetGame()"
    :clickedLetters = "clickedLetters"
    />
    <Board
    v-if="this.chosenWord"
    @clicked-letters="setLetters"
    :chosenWord = "chosenWord"
    ref="resetTrigger"
    />
  </v-app>
</template>

<script>
import Header from './components/Header.vue'
import Board from './components/Board.vue'

export default {
  name: 'App',
  components: {
    Header,
    Board
  },
  methods: {
    setLetters(letters) {
      this.clickedLetters = letters.join("");
    },
    onChosenWord(word) {
      this.chosenWord = word.join("");
    },
  },
  data () {
    return {
      clickedLetters : '',
      chosenWord : '',
    }
  }
}
</script>

<style lang="scss">

:root {
  --baseSize: 4.5em;
}

#app {
  .v-application {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    background-color: #ebf2fa;
  }
}


</style>
