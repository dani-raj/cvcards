<template>
  <div class="app">
    <div v-if="showFullscreenMsg" >
      <FullscreenMessage :screen='screen' @startGame="startGame" />
    </div>
      <Countdown @gameEnd="setGameOver" :canStart="canStart" />
      <Cards @gameEnd="setGameOver" />
  </div>
</template>

<script>
import FullscreenMessage from './components/FullscreenMessage.vue'
import Countdown from './components/Countdown.vue'
import Cards from './components/Cards.vue'

export default {
  name: 'App',
  components: {
    FullscreenMessage,
    Countdown,
    Cards
  },
  data() {
    return {
      showFullscreenMsg: true,
      screen: 'Start',
      canStart: false,
    }
  },
  methods: {
    startGame() {
      this.showFullscreenMsg = false
      this.canStart = true
    },
    setGameOver(value) {
      this.showFullscreenMsg = true
      this.screen = value
      this.canStart = false
    },
  }
}
</script>

<style>
  @font-face{
    font-family: "Rubik", sans-serif;
    src: url("https://fonts.googleapis.com/css2?family=Rubik:wght@300;400;500;600;800&display=swap")
  }
  *{
    box-sizing: border-box;
  }
  html{
    font-family: 'Rubik', sans-serif;
    min-height: 100vh;
  }
  body{
    margin: 0;
    background: radial-gradient(#13023e, #00031e);
  }

</style>
