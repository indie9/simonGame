<template>
  <button class="button" :class="{ 'button--disabled': disabled }" @click="handleClick">
    <slot></slot>
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleClick() {
      if (!this.disabled) {
        this.$emit('click');
      }
    }
  }
};
</script>

<style lang="sass">
  @import "index.sass"
</style>
