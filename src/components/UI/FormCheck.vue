<template>
  <div class="checkbox">
    <label>
      <input type="checkbox" v-model="isChecked" @change="updateValue" />
      {{ label }}
      <span v-if="isRequired" class="checkbox__star">*</span>
    </label>
    <p
      v-if="validate?.[name]?.$error && !validate?.[name]?.required"
      class="invalid-feedback"
    >
      {{ errorMessage }}
    </p>
  </div>
</template>

<script>
export default {
  props: {
    label: {
      type: String,
      required: true,
    },
    errorMessage: {
      type: String,
      default: "This field is required",
    },
    name: {
      type: String,
      required: true,
    },
    isRequired: {
      type: Boolean,
      default: false,
    },
    validate: {
      type: Object,
    },
  },
  data() {
    return {
      isChecked: false,
    };
  },
  methods: {
    updateValue() {
      this.$emit("change", this.name, this.isChecked);
    },
  },
};
</script>

<style lang="scss" scoped>
.checkbox {
  font-weight: bold;

  label {
    display: flex;
    align-items: center;
  }

  .is-invalid {
    border-color: red;
  }

  .invalid-feedback {
    color: red;
    margin-top: 5px;
  }

  &__star {
    color: red;
  }
}
</style>
