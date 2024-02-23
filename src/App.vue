<template>
  <div id="app">
    <form-input label="Имя" :is-required="true"></form-input>
  </div>
</template>

<script>
import FormInput from "@/components/UI/FormInput.vue";

export default {
  name: "App",
  components: { FormInput },
};
</script>

<style lang="scss"></style>
