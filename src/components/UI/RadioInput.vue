<template>
  <div class="radio-input">
    <div class="radio-input__title">
      Пол:
      <span v-if="isRequired" class="radio-input__star">*</span>
    </div>
    <div
      class="radio-input__item"
      v-for="(option, index) in options"
      :key="index"
    >
      <input
        :id="'radioInput' + index"
        type="radio"
        :value="option.value"
        @change="updateValue"
        v-model="selectedGender"
      />
      <label :for="'radioInput' + index">{{ option.label }}</label>
    </div>
    <span
      v-if="validation?.[name]?.$dirty && !validation?.[name]?.required"
      class="radio-input__error"
    >
      {{ errorMessage }}
    </span>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";
export default {
  props: {
    errorMessage: {
      type: String,
      default: "Это поле обязательное, выберите значение",
    },
    options: {
      type: Array,
      required: true,
    },
    name: {
      type: String,
      required: true,
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
      selectedGender: "",
    };
  },
  validations: {
    selectedGender: {
      required,
    },
  },
  methods: {
    updateValue() {
      this.$emit("change", this.name, this.selectedGender);
    },
  },
};
</script>

<style lang="scss" scoped>
.radio-input {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  font-weight: bold;

  &__title {
    margin-bottom: 5px;
  }

  label {
    margin-bottom: 5px;
  }

  &__item {
    display: flex;
    align-items: flex-start;
  }

  &__star {
    color: red;
  }

  &__error {
    color: red;
  }
}
</style>
