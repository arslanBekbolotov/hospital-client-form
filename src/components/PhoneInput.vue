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
      @focus="validation?.[name]?.$reset()"
      :class="{
        'form-input__field': true,
        'form-input__field--invalid': validation?.[name]?.$error,
      }"
    />
    <div
      :class="['form-input__error', { visible: validation?.[name]?.$error }]"
    >
      <div v-if="validation?.[name]?.$dirty && !validation?.[name]?.required">
        {{ errorMessage }}
      </div>
      <div v-if="validation?.[name]?.$dirty && !validation?.[name]?.minLength">
        Номер должен состоять из 11 цифр
      </div>
      <div v-if="validation?.[name]?.$dirty && !validation?.[name]?.validPhone">
        Номер должен начинаться с цифры 7 (формат записи: 7123456789)
      </div>
    </div>
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
      default: "Это поле обязательное",
    },
    validation: {
      type: Object,
    },
  },
  data() {
    return {
      localValue: "",
      inputId: `inputId ${Date.now() + Math.random()}`,
    };
  },
  methods: {
    updateValue() {
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
  font-weight: bold;
  width: 100%;

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
    color: red;
    text-align: start;
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
