<template>
  <div class="form-input__item">
    <label :for="inputId" class="form-input__label">
      {{ label }}
      <span v-if="isRequired" class="form-input__star">*</span>
    </label>
    <input
      :id="inputId"
      :type="type"
      v-model.trim="localValue"
      :placeholder="placeholder"
      @input="$v.localValue.$touch()"
      @blur="$v.localValue.$touch()"
      @focus="$v.localValue.$reset()"
      :class="{ 'form-input__field--invalid': $v.localValue.error }"
      class="form-input__field"
    />
    <span
      v-show="$v.localValue.$dirty && !$v.localValue.required"
      class="form-input__error"
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
    type: {
      type: String,
      default: "text",
    },
    value: {
      type: [String, Number],
      default: "",
    },
    placeholder: {
      type: String,
      default: "",
    },
    isRequired: {
      type: Boolean,
      default: false,
    },
    showError: {
      type: Boolean,
      default: false,
    },
    errorMessage: {
      type: String,
      default: "Пожалуйста введите корректное значение",
    },
  },
  data() {
    return {
      localValue: this.value,
      inputId: `inputId ${Date.now() + Math.random()}`,
    };
  },
  validations: {
    localValue: {
      required,
    },
  },
  watch: {
    value(newValue) {
      this.localValue = newValue;
    },
  },
  methods: {},
};
</script>

<style scoped>
.form-input__item {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.form-input__label {
  font-weight: bold;
  align-self: flex-start;
}

.form-input__star {
  color: red;
}

.form-input__field {
  width: 100%;
  padding: 10px;
  margin: 5px 0 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

.form-input__field--invalid {
  border-color: red;
}

.form-input__error {
  color: red;
}
</style>
