<template>
  <button class="button">
    <span class="button__text">{{ buttonText }}</span>
    <slot class="button__icon" />
  </button>
</template>

<script>
export default {
  props: {
    buttonText: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss">
.button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 10px 20px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  color: #fff;
  transition: background-color 0.3s ease;
  background-color: #3974c2;
  font-size: 20px;
  font-weight: bold;

  &__text {
    margin-right: 8px;
  }

  &__icon {
    font-size: 20px;
    width: 25px;
    height: auto;
  }
}
</style>
