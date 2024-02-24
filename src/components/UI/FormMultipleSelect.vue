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
      :class="[
        'multi-select-dropdown__error',
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
    defaultValue: {
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
      inputId: `multiSelectId ${Date.now() + Math.random()}`,
      selectedOptions: [],
      isOpen: false,
    };
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
.multi-select-dropdown {
  display: flex;
  align-items: flex-start;
  flex-direction: column;
  min-width: 280px;
  font-weight: bold;
  width: 100%;

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
    background: #fff;
    border: 1px solid #ccc;
    border-radius: 5px;
    color: #000;
  }

  &__selected {
    width: 100%;
    padding: 10px;
    box-sizing: border-box;
    position: relative;
    z-index: 1;

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
    text-align: center;
    z-index: 1;
    border: 1px solid #ccc;
    border-radius: 5px;
    box-sizing: border-box;

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
    text-align: start;
    margin-top: 5px;
    opacity: 0;
    transform: translateY(15px);
    transition: opacity 0.2s ease, transform 0.2s ease;

    &.visible {
      opacity: 1;
      transform: translateY(0);
    }
  }
}
</style>
