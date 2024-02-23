<template>
  <div class="multi-select-dropdown">
    <label :for="inputId" class="multi-select-dropdown__label">
      {{ label }}
      <span v-if="isRequired" class="multi-select-dropdown__star">*</span>
    </label>
    <div class="multi-select-dropdown__wrapper" @click="toggleDropdown">
      <div
        class="multi-select-dropdown__selected"
        :class="{ 'is-open': isOpen }"
      >
        {{ getSelectedLabels() || defaultValue }}
      </div>
      <div v-show="isOpen" class="multi-select-dropdown__options">
        <div
          v-for="(option, index) in options"
          :key="index"
          @click="changeOption(option)"
        >
          {{ option.label }}
        </div>
      </div>
    </div>
    <span
      v-if="$v.selectedOptions.$dirty && !$v.selectedOptions.required"
      class="multi-select-dropdown__error"
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
      default: "Выберите...",
    },
    errorMessage: {
      type: String,
      default: "Пожалуйста, выберите значение",
    },
    isRequired: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      inputId: `multiSelectId ${Date.now() + Math.random()}`,
      selectedOptions: [],
      isOpen: false,
    };
  },
  validations: {
    selectedOptions: {
      required,
    },
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
    },
    changeOption(option) {
      const index = this.selectedOptions.findIndex(
        (selectedOption) => selectedOption.value === option.value
      );

      if (index === -1) {
        this.selectedOptions.push(option);
      } else {
        this.selectedOptions.splice(index, 1);
      }

      this.$emit("change", this.name, this.selectedOptions);
    },
    getSelectedLabels() {
      return this.selectedOptions.map((option) => option.label).join(", ");
    },
  },
};
</script>

<style lang="scss" scoped>
.multi-select-dropdown {
  display: flex;
  align-items: flex-start;
  flex-direction: column;

  &__label {
    font-weight: bold;
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
  }

  &__selected {
    width: 100%;
    padding: 10px;
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
    overflow: hidden;
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
    text-align: center;
    z-index: 1;

    div {
      padding: 10px;
      cursor: pointer;

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
