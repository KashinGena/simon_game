<template>
  <div class="game__wrapper">
    <div class="game-field">
      <div
        class="game-field__box game-field__box_blue"
        @click="doStep(0)"
        :class="{ light: isLight[0] }"
      ></div>
      <div
        class="game-field__box game-field__box_red"
        @click="doStep(1)"
        :class="{ light: isLight[1] }"
      ></div>
      <div
        class="game-field__box game-field__box_yellow"
        @click="doStep(2)"
        :class="{ light: isLight[2] }"
      ></div>
      <div
        class="game-field__box game-field__box_green"
        @click="doStep(3)"
        :class="{ light: isLight[3] }"
      ></div>

      <button class="game__btn" @click="Start">{{ buttonMode }}</button>
    </div>

    <div class="game-options">
      <h2 class="game-options__title" v-show="isPlaying">
        {{ title }}
      </h2>

      <div class="game-options__level game-level" v-show="!isPlaying">
        <h3 class="game-level__title">Choose level</h3>
        <label class="game-level__option">
          <input value="easy" checked v-model="level" type="radio" />
          Easy
        </label>
        <label class="game-level__option">
          <input value="medium" v-model="level" type="radio" />
          Medium
        </label>
        <label class="game-level__option">
          <input value="hard" v-model="level" type="radio" />
          Hard
        </label>
      </div>
    </div>
    <div class="audio">
      <audio ref="audio4" preload="auto">
        <source src="sounds/2.ogg" type="audio/ogg" />
        <source src="sounds/2.mp3" type="audio/mp3" />
      </audio>
      <audio ref="audio1" preload="auto">
        <source src="sounds/1.ogg" type="audio/ogg" />
        <source src="sounds/1.mp3" type="audio/mp3" />
      </audio>
      <audio ref="audio2" preload="auto">
        <source src="sounds/2.ogg" type="audio/ogg" />
        <source src="sounds/2.mp3" type="audio/mp3" />
      </audio>
      <audio ref="audio3" preload="auto">
        <source src="sounds/3.ogg" type="audio/ogg" />
        <source src="sounds/3.mp3" type="audio/mp3" />
      </audio>
    </div>
  </div>
 
</template>

<script>


export default {
  data() {
    return {
      sequence: [],
      playerSequence: [],
      round: 0,
      isClickable: false,
      isPlaying: false,
      level: "easy",
      isWrong: false,
      isLight: { 0: false, 1: false, 2: false, 3: false },
      buttonMode: "Start",
      timerId: null,
    };
  },

  computed: {
    duration() {
      switch (this.level) {
        case "easy":
          return 1500;
          break;
        case "medium":
          return 1000;
          break;
        case "hard":
          return 400;
          break;

        default:
          return 1000;
          break;
      }
    },

    title() {
      if (this.isWrong) {
        return "Game Over !";
      } else {
        return `Round: ${this.round}`;
      }
    },
  },

  methods: {

    Start() {
      if (this.buttonMode === "Start") {
        this.buttonMode = "Reset";
        this.isPlaying = true;
        this.showSequence();
      } else {
        this.Reset();
      }
    },

    showSequence() {
      this.isClickable = false;
      this.sequence.push(Math.floor(Math.random() * Math.floor(4)));
      this.playerSequence = [];
      const to = this.sequence.length - 1;
      let current = 0;
      const self = this;
      this.timerId = setInterval(() => {
        this.playSound(this.sequence[current]);
        this.lightBox(this.sequence[current]);

        if (current >= to) {
          this.isClickable = true;
          clearInterval(this.timerId);
        }
        current++;
      }, this.duration);
    },

    lightBox(number) {
      this.isLight[number] = true;
      setTimeout(() => {
        this.isLight[number] = false;
      }, 350);
    },

    playSound(number) {
      switch (number) {
        case 0:
          if (this.level=='hard')
            this.$refs.audio1.currentTime = 0.4
          this.$refs.audio1.play()
          break

        case 1:
          if (this.level=='hard')
            this.$refs.audio2.currentTime = 0.4
          this.$refs.audio2.play()
          break

        case 2: {
          if (this.level=='hard')
            this.$refs.audio3.currentTime = 0.4
          this.$refs.audio3.play()
          break
        }

        case 3: {
          if (this.level=='hard')
            this.$refs.audio4.currentTime = 0.4
          this.$refs.audio4.play()
          break
        }

        default:
          break
      }
    },

    doStep(index) {
      if (this.isClickable) {
        this.playerSequence.push(index);
        this.playSound(index);
        this.lightBox(index);
        const step = this.playerSequence.length - 1;
        if (this.checkStep(step)) {
          if (this.sequence.length === this.playerSequence.length) {
            this.playerSequence = [];
            this.round += 1;
            setTimeout(() => this.showSequence(), 500);
          }
        } else {
          this.isWrong = true
          setTimeout(() => this.Reset(), 1500);
        }
      }
    },

    checkStep(index) {
      return this.sequence[index] === this.playerSequence[index];
    },

    Reset() {
      this.isPlaying = false;
      this.isWrong = false;
      this.buttonMode = "Start";
      clearInterval(this.timerId);
      this.sequence = [];
      this.playerSequence = [];
      this.round = 0;
      this.isClickable = false;
      this.level = "easy";
    },
  }


};
</script>

<style lang="scss">

.game-field {
  display: -moz-grid;
  display: -ms-grid;
  display: grid;
  width: 300px;
  height: 300px;
  grid-template-columns: repeat(2, 1fr);
  grid-template-rows: repeat(2, 1fr);
  position: relative;
  border-radius: 100%;
  border: 1px solid #000;

  &__box {
    opacity: 0.5;
    &_blue {
      grid-column-start: 1;
      grid-column-end: 2;
      grid-row-start: 1;
      grid-row-end: 2;
      border-top-left-radius: 100%;
      background-color: blue;
      &.light {
        opacity: 1;
      }
    }

    &_red {
      grid-column-start: 2;
      grid-column-end: 3;
      grid-row-start: 1;
      grid-row-end: 2;
      border-top-right-radius: 100%;
      background-color: red;
      &.light {
        opacity: 1;
      }
    }

    &_yellow {
      grid-column-start: 1;
      grid-column-end: 2;
      grid-row-start: 2;
      grid-row-end: 3;
      border-bottom-left-radius: 100%;
      background-color: yellow;
      &.light {
        opacity: 1;
      }
    }

    &_green {
      grid-column-start: 2;
      grid-column-end: 3;
      grid-row-start: 2;
      grid-row-end: 3;
      border-bottom-right-radius: 100%;

      background-color: green;
      &.light {
        opacity: 1;
      }
    }
  }
}

.game-level {
  width: 250px;

  &__title {
    display: block;
    font-size: 1.5em;
    margin-block-start: 0.83em;
    margin-block-end: 0.83em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    font-weight: bold;
  }
}

.game__btn {
  width: 3.5em;
  height: 3.5em;
  box-sizing: border-box;
  font-size: 1.4em;
  text-align: center;
  background: #6dabe8;
  -webkit-border-radius:100%;
  border-radius: 100%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  border: 1px solid #000;
  transition: background-color 0.5s linear;
  outline: none;

  &:active {
    background-color: darken(#6dabe8, 40);
  }
}

.game-level {
  &__title {
    font-size: 1.5em;
  }
  &__option {
    font-size: 1em;
    & input {
      width: 20px;
      height: 20px;
    }
  }
}

@media (max-width: 600px) {
  .game__wrapper {
    flex-direction: column-reverse;
    align-items: center;
  }
  .game-options {
    margin-bottom: 20px;
    height: 88px;

    &__title {
      margin-top: 50px;
    }
  }
}
</style>
