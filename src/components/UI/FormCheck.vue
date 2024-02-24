<template>
  <div class="checkbox">
    <label>
      <input type="checkbox" v-model="isChecked" @change="updateValue" />
      {{ label }}
      <span v-if="isRequired" class="checkbox__star">*</span>
    </label>
    <p v-if="$v.isChecked.$error" class="invalid-feedback">
      {{ errorMessage }}
    </p>
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
  },
  data() {
    return {
      isChecked: false,
    };
  },
  validations: {
    isChecked: {
      required,
    },
  },
  methods: {
    updateValue() {
      this.$v.isChecked.$touch();
      this.$emit("change", this.name, this.isChecked);
    },
  },
};
</script>

<style lang="scss" scoped>
.checkbox {
  label {
    display: flex;
    align-items: center;
    font-weight: bold;
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
