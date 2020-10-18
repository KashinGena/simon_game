<template>
  <div class="modal">
    <div class="modal__wrapper">
      <h2>Правила</h2>
      <ul class="rule-list">
        <li class="rule-list__item">Для начала игры выберите уровень сложности и нажмите Start</li>
        <li class="rule-list__item">Разноцветные кнопки-панели будут загораться</li>
        <li class="rule-list__item">Ваша задача - воспроизвести эту последовательность</li>
        <li class="rule-list__item">Если Вы нажали правильно, то последовательность увеличивается</li>
        <li class="rule-list__item">Если ошиблись, то конец игры</li>
        <li class="rule-list__item">Для сброса игры нажмите Reset</li>
      </ul>
      <button class="modal__btn" @click="onClose">Я все понял!</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    round: {
      type: Number,
    },
  },

  methods: {
    onClose() {
      this.$emit("onClose");
    },
  },
};
</script>

<style lang="scss">
.modal {
  min-width: 60%;
  position: absolute;
  z-index: 999;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  border: 2px solid blue;
  border-radius: 10px;
}

.modal__wrapper {
    padding: 10px;
}
.rule-list {
    text-align: left;
    font-size: 1em;
    margin-bottom: 40px;
}
.rule-list__item {
    margin-bottom: 10px;
    font-size: 20px;

}
.modal__btn {
  width: 150px;
  box-sizing: border-box;
  font-size: 20px;
  text-align: center;
  background: #6dabe8;
  -webkit-border-radius:5px;
  border-radius: 5px;
 padding: 3px 5px;
  transition: background-color 0.5s linear;
  outline: none;

  &:active {
    background-color: darken(#6dabe8, 40);
  }
}


@media (max-width:500px) {
    .modal {
         width: 90%;
    }
}
</style>
