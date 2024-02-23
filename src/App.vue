<template>
  <div id="app">
    <form-input
      label="Имя"
      :is-required="true"
      name="name"
      @input="updateParentValue"
      placeholder="Иван"
    />
    <form-input
      label="Дата рождения"
      @blur="updateParentValue"
      type="date"
      name="birthdate"
      :is-required="true"
    />
    <phone-input name="phone" label="Номер телефона" :is-required="true" />
    <form-select
      default-value="Выберите..."
      :options="doctors"
      name="selectedDoctor"
      label="Лечащий врач"
      @change="updateParentValue"
      :is-required="true"
    />
    <form-multiple-select
      default-value="Выберите..."
      :options="groups"
      name="selectedGroups"
      label="Группа клиентов"
      :is-required="true"
      @change="updateParentValue"
    />
  </div>
</template>

<script>
import FormInput from "@/components/UI/FormInput.vue";
import PhoneInput from "@/components/PhoneInput.vue";
import FormSelect from "@/components/UI/FormSelect.vue";
import FormMultipleSelect from "@/components/UI/FormMultipleSelect.vue";

export default {
  name: "App",
  components: { FormMultipleSelect, FormSelect, PhoneInput, FormInput },
  data() {
    return {
      name: "",
      lastname: "",
      birthdate: "",
      phone: "",
      clientGroup: [],
      attendingDoctor: [],
      agreeWithSendSms: false,
      selectedGroups: [],
      selectedDocument: "",
      selectedDoctor: null,
      city: "",
      identification: [],
      issueDate: "",
      successMsg: false,
      groups: [
        {
          label: "VIP",
          value: "VIP",
        },
        {
          label: "Проблемные",
          value: "Проблемные",
        },
        {
          label: "ОМС",
          value: "ОМС",
        },
      ],
      doctors: [
        {
          label: "Иванов",
          value: "Ivanov",
        },
        {
          label: "Захаров",
          value: "Zaharov",
        },
        {
          label: "Чернышева",
          value: "Chernisheva",
        },
      ],
      documents: [
        {
          label: "Паспорт",
          value: "Passport",
        },
        {
          label: "Свидетельство о рождении",
          value: "BC",
        },
        {
          label: "Вод. удостоверение",
          value: "driversLicense",
        },
      ],
    };
  },
  methods: {
    updateParentValue(name, newValue) {
      this[name] = newValue;
    },
  },
};
</script>

<style lang="scss"></style>
