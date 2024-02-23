<template>
  <div class="custom-dropdown">
    <label :for="inputId" class="custom-dropdown__label">
      {{ label }}
    </label>
    <div class="custom-dropdown__wrapper" @click="toggleDropdown">
      <div class="custom-dropdown__selected" :class="{ 'is-open': isOpen }">
        {{ selectedValue.label || defaultValue || "Выберите..." }}
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
      v-if="$v.selectedValue.$dirty && !$v.selectedValue.required"
      class="custom-dropdown__error"
    >
      {{ errorMessage }}
    </span>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";

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
    defaultValue: {
      type: String,
      required: true,
    },
    errorMessage: {
      type: String,
      default: "Пожалуйста, выберите значение",
    },
  },
  data() {
    return {
      inputId: `dropdownId ${Date.now() + Math.random()}`,
      selectedValue: this.options,
      isOpen: false,
    };
  },
  validations: {
    selectedValue: {
      required,
    },
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    selectOption(option) {
      this.selectedValue = option;
      this.isOpen = false;
      this.$emit("change", this.name, option);
      this.isOpen = true;
    },
  },
};
</script>

<style lang="scss" scoped>
.custom-dropdown {
  display: flex;
  align-items: flex-start;
  flex-direction: column;

  &__label {
    font-weight: bold;
    margin-bottom: 5px;
  }

  &__wrapper {
    position: relative;
    display: inline-block;
    width: 100%;
    cursor: pointer;
  }

  &__selected {
    width: 100%;
    padding: 10px;
    margin: 5px 0 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;
    position: relative;

    &.is-open {
      border-bottom-left-radius: 0;
      border-bottom-right-radius: 0;
    }
  }

  &__options {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    background-color: #fff;
    border: 1px solid #ccc;
    border-top: none;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;
    box-sizing: border-box;
    z-index: 1;

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
    margin-top: 5px;
  }
}
</style>
