<template>
  <div
    :class="['alert', { 'alert--error': error }, { 'alert--visible': visible }]"
  >
    <span>{{ message }}</span>
    <div class="alert__line" v-if="visible"></div>
  </div>
</template>

<script>
export default {
  props: {
    error: {
      type: Boolean,
      default: false,
    },
    message: {
      type: String,
      default: "",
    },
    visible: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style lang="scss">
.alert {
  padding: 10px;
  border-radius: 5px;
  position: fixed;
  opacity: 0;
  transition: opacity 1s ease-in-out;
  background-color: #4caf50;
  top: 20px;
  right: 20px;
  font-size: 18px;
  font-weight: bold;
  color: #fff;
  z-index: 2;
  margin-bottom: 10px;

  &--error {
    background-color: #f44336;
  }

  &--visible {
    opacity: 1;
  }

  &__line {
    height: 4px;
    background-color: #a167e7;
    border: 1px solid #3498db;
    border-radius: 2px;
    animation: fillLine 5s ease;
  }

  @keyframes fillLine {
    0% {
      width: 0;
    }
    100% {
      width: 100%;
    }
  }
}
</style>
