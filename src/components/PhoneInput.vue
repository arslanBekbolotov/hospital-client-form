<template>
  <div class="form-input__item">
    <label :for="inputId" class="form-input__label">
      {{ label }}
      <span v-if="isRequired" class="form-input__star">*</span>
    </label>
    <input
      :id="inputId"
      type="tel"
      v-model.trim="localValue"
      :placeholder="placeholder"
      @blur="updateValue"
      maxlength="11"
      @focus="$v.localValue.$reset()"
      :class="{
        'form-input__field': true,
        'form-input__field--invalid': $v.localValue.$error,
      }"
    />
    <div class="form-input__error">
      <div v-if="$v.localValue.$dirty && !$v.localValue.required">
        {{ errorMessage }}
      </div>
      <div v-if="$v.localValue.$dirty && !$v.localValue.minLength">
        Номер должен состоять из 11 цифр
      </div>
      <div v-if="$v.localValue.$dirty && !$v.localValue.validPhone">
        Номер должен начинаться с цифры 7 (формат записи: 7123456789)
      </div>
    </div>
  </div>
</template>

<script>
import { required, minLength } from "vuelidate/lib/validators";

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
    placeholder: {
      type: String,
      default: "",
    },
    isRequired: {
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
      localValue: "",
      inputId: `inputId ${Date.now() + Math.random()}`,
    };
  },
  validations: {
    localValue: {
      required,
      minLength: minLength(11),
      validPhone: (val) => /^7\d{10}$/.test(val),
    },
  },
  methods: {
    updateValue() {
      this.$v.localValue.$touch();
      this.$emit("blur", this.name, this.localValue);
    },
  },
};
</script>

<style lang="scss" scoped>
.form-input__item {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  width: 100%;

  .form-input__label {
    font-weight: bold;
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

    &--invalid {
      border-color: red;
    }
  }

  .form-input__error {
    text-align: start;
    color: red;
  }
}
</style>
