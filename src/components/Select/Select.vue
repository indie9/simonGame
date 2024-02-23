<template>
  <div class="select">
    <div class="select__wrapper">
      <div class="select__selected-option" @click="toggleDropdown" :class="{ 'select__selected-option--error': isError }">
        {{ value ? value.label : "Выберите значение" }}
        <svg class="select__arrow" :class="{ 'select__arrow--rotated': isOpen }" xmlns="http://www.w3.org/2000/svg"
          viewBox="0 0 24 24">
          <path d="M7 10l5 5 5-5z" />
        </svg>
      </div>
      <ul v-show="isOpen" class="select__options">
        <li v-for="option in options" :key="option.id" :class="{ 'select__options--active': value == option }"
          @click="selectOption(option)">
          {{ option.label }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Select",
  data() {
    return {
      isOpen: false,
    };
  },
  props: {
    options: {
      type: Array,
      required: true,
    },
    isError: Boolean,
    value: Object,
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    selectOption(option) {
      this.isOpen = false;
      this.$emit("input", option);
    },
  },
};
</script>

<style lang="sass" scoped>
  @import "index.sass"
</style>
