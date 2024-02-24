<template>
  <div class="custom-dropdown">
    <label :for="inputId" class="custom-dropdown__label">
      {{ label }}
      <span v-if="isRequired" class="custom-dropdown__star">*</span>
    </label>
    <div class="custom-dropdown__wrapper" @click="toggleDropdown">
      <div class="custom-dropdown__selected" :class="{ 'is-open': isOpen }">
        {{ selectedValue.label || startValue }}
      </div>
      <div v-show="isOpen" class="custom-dropdown__options">
        <div
          v-for="(option, index) in options"
          :key="index"
          @click="selectOption(option)"
        >
          {{ option.label }}
        </div>
      </div>
    </div>
    <span
      :class="[
        'custom-dropdown__error',
        { visible: validation?.[name]?.$error },
      ]"
    >
      {{ errorMessage }}
    </span>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    name: {
      type: String,
      required: true,
    },
    options: {
      type: Array,
      required: true,
    },
    startValue: {
      type: String,
      default: "Выберите...",
    },
    errorMessage: {
      type: String,
      default: "Это поле обязательное, выберите значение",
    },
    isRequired: {
      type: Boolean,
      default: false,
    },
    validation: {
      type: Object,
    },
  },
  data() {
    return {
      inputId: `dropdownId ${Date.now() + Math.random()}`,
      selectedValue: "",
      isOpen: false,
    };
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    selectOption(option) {
      this.selectedValue = option;
      this.isOpen = false;
      this.$emit("change", this.name, option);
    },
  },
  created() {
    const onClickOutside = (e) =>
      (this.isOpen = this.$el.contains(e.target) && this.isOpen);
    document.addEventListener("click", onClickOutside);
    this.$on("hook:beforeDestroy", () =>
      document.removeEventListener("click", onClickOutside)
    );
  },
};
</script>

<style lang="scss" scoped>
.custom-dropdown {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  font-weight: bold;

  &__label {
    margin-bottom: 5px;
  }

  &__star {
    color: red;
  }

  &__wrapper {
    position: relative;
    display: inline-block;
    width: 100%;
    cursor: pointer;
    border: 1px solid #ccc;
    background: #fff;
    border-radius: 10px;
    color: #000;
  }

  &__selected {
    width: 100%;
    padding: 10px;
    position: relative;

    &.is-open {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }
  }

  &__options {
    position: absolute;
    overflow: hidden;
    top: 100%;
    left: 0;
    width: 100%;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    background-color: #fff;
    border: 1px solid #ccc;
    z-index: 1;
    border-radius: 5px;
    box-sizing: border-box;

    div {
      padding: 10px;
      cursor: pointer;
      transition: background-color 0.3s;

      &:hover {
        background-color: #f2f2f2;
        transform: scale(1.1);
      }
    }
  }

  &__error {
    color: red;
    text-align: start;
    margin-top: 5px;
    opacity: 0;
    transform: translateY(10px);
    transition: opacity 0.2s ease, transform 0.2s ease;

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
</style>
