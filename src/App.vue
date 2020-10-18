<template>
  <div id="app">
    <div class="app__container">
      <header>
        <h1 class="app__title">
          Simon Game
          <button @click="isHelp = true" class="help">(how to play)</button>
        </h1>
      </header>

      <Game />
      <Modal v-if="isHelp" @onClose="onModalClose" />
    </div>
  </div>
</template>

<script>
import Game from "./components/Game.vue";
import Modal from "./components/Modal.vue";

export default {
  name: "App",
  data() {
    return {
      isHelp: false,
    };
  },
  components: {
    Game,
    Modal,
  },

  methods: {
    onModalClose() {
      this.isHelp = false;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.app__container {
  max-width: 600px;
  margin: 0 auto;
}

.app__title {
  margin-bottom: 50px;
  text-align: center;
}
.help {
  border: none;
  outline: none;
  font-size: inherit;
  background: inherit;
  color: blue;
  margin-left: 20px;
  cursor: pointer;
}

.game__wrapper {
  display: flex;
  justify-content: space-between;
}
@media (max-width: 600px) {
  .app__title {
    margin-bottom: 10px;
  }
}
@media (max-width: 436px) {
  .help {
    margin-left: 0;
  }
}
</style>
