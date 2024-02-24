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
      @blur="updateValue"
      @focus="validation?.[name]?.$reset()"
      :class="{
        'form-input__field': true,
        'form-input__field--invalid': validation?.[name]?.$error,
      }"
      :max="getNowDate()"
    />
    <span
      :class="['form-input__error', { visible: validation?.[name]?.$error }]"
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
    type: {
      type: String,
      default: "text",
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
      this.$emit("input", this.name, this.localValue);
    },

    getNowDate() {
      const date = new Date();
      return `${date.getFullYear()}-${String(date.getMonth() + 1).padStart(
        2,
        "0"
      )}-${String(date.getDate()).padStart(2, "0")}`;
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
    padding: 10px;
    width: 100%;
    margin: 5px 0 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;

    &--invalid {
      border: 1px solid red;
    }
  }

  .form-input__error {
    color: red;
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
