<template>
  <div class="game">
    <div class="game__board">
      <div v-for="(button, index) in buttons" :key="index" class="game__button"
        :class="{ active: button.active, [colors[index]]: true }" @click="buttonClicked(index)"></div>
      <Button @click="handleClick">{{
        isGame ? "начать заново" : "старт"
      }}</Button>
      <Select :options="speedArray" v-model="speed" />
      <div class="game__result">Текущий уровень: {{ arrayButton.length }}</div>
      <FormResult v-model="isOpenForm" :level="level" />
    </div>
  </div>
</template>

<script>
import Button from "@/components/Button/Button.vue";
import Select from "@/components/Select/Select.vue";
import FormResult from "@/components/FormResult/FormResult.vue";

import simonSound1 from "@/assets/simonSound1.mp3";
import simonSound2 from "@/assets/simonSound2.mp3";
import simonSound3 from "@/assets/simonSound3.mp3";
import simonSound4 from "@/assets/simonSound4.mp3";

export default {
  components: {
    Button,
    Select,
    FormResult,
  },
  data() {
    return {
      buttons: [
        { id: 0, active: false },
        { id: 1, active: false },
        { id: 2, active: false },
        { id: 3, active: false },
      ],
      arrayButton: [],
      isOn: false,
      isGame: false,
      counter: 0,
      colors: ["red", "green", "yellow", "blue"],
      speed: { id: 0, label: "легкий"},
      speedArray: [
        { id: 0, label: "легкий"},
        { id: 1, label: "нормальный"},
        { id: 2, label: "сложный"}
      ],
      audio: [
        new Audio(simonSound1),
        new Audio(simonSound2),
        new Audio(simonSound3),
        new Audio(simonSound4),
      ],
      isOpenForm: false,
      level: 1,
    };
  },
  computed: {
    getSpeed() {
      switch (this.speed.id) {
        case 0:
          return 1500;
        case 1:
          return 1000;
        case 2:
          return 400;
        default:
          return 1500;
      }
    },
  },
  methods: {
    async handleClick() {
      this.counter = 0;
      this.arrayButton = [];
      this.startCycle();
      this.isGame = true;
    },

    async startCycle() {
      this.isOn = true;
      this.arrayButton.push(Math.floor(Math.random() * 4));
      for (let index = 0; index < this.arrayButton.length; index++) {
        this.audio[this.arrayButton[index]].play();
        await this.lightButton(this.arrayButton[index], 500)
        if (index != this.arrayButton.length - 1)
          await this.delay(this.getSpeed);

      }
      this.isOn = false;
    },

    delay(ms) {
      return new Promise((resolve) => setTimeout(resolve, ms));
    },

    async lightButton(index, time) {
      this.buttons[index].active = true;
      await this.delay(time);
      this.buttons[index].active = false;
    },
    async buttonClicked(index) {
      if (this.isOn || !this.isGame) return;
      this.counter++;
      this.audio[index].play();
      if (this.arrayButton[this.counter - 1] == this.buttons[index].id) {
        this.lightButton(index, 500)

        if (this.counter == this.arrayButton.length) {
          await this.delay(1500);
          this.counter = 0;
          this.startCycle();
          return;
        }
      } else {
        this.isGame = false;
        await this.lightButton(index, 500)
        this.isOpenForm = true;
        this.counter = 0;
        this.level = this.arrayButton.length - 1;
        this.arrayButton = [];
      }
    },
  },
  watch: {
    speed: function () {
      this.isGame = false;
        this.counter = 0;
        this.level = 0;
        this.arrayButton = [];
    },
  },
};
</script>

<style lang="sass" scoped>
  @import "index.sass"
</style>

