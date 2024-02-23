<template>
  <div class="number-form" v-if="value" @click.self="closeForm">
    <div class="number-form__container">
      <p>Вы смогли возвроизвести последовательность длиною {{ level }}</p>
      <Button @click="closeForm">ок</Button>
    </div>
  </div>
</template>

<script>
import Button from "@/components/Button/Button.vue";

export default {
  name: "FormResult",
  components: {
    Button,
  },
  props: {
    value: {
      type: Boolean,
      default: false,
    },
    level: Number,
  },
  methods: {
    closeForm() {
      this.$emit("input", false);
    },
  },
};
</script>

<style lang="sass" scoped>
  @import "index.sass"
</style>
