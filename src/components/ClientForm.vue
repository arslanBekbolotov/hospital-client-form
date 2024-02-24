<template>
  <form class="form">
    <div class="main-form">
      <form-input
        label="Имя"
        :is-required="true"
        name="name"
        @input="updateParentValue"
      />
      <form-input
        label="Фамилия"
        :is-required="true"
        name="lastname"
        @input="updateParentValue"
      />
      <form-input
        label="Дата рождения"
        @blur="updateParentValue"
        type="date"
        name="birthdate"
        :is-required="true"
      />
      <phone-input name="phone" label="Номер телефона" :is-required="true" />
      <div class="main-form__row">
        <radio-input
          :options="genderOption"
          @change="updateParentValue"
          name="selectedGender"
        />
        <form-multiple-select
          style="flex-grow: 1"
          :options="groups"
          name="selectedGroups"
          label="Группа клиентов"
          :is-required="true"
          @change="updateParentValue"
        />
      </div>
      <form-select
        :options="doctors"
        name="selectedDoctor"
        label="Лечащий врач"
        @change="updateParentValue"
      />
      <form-check
        @change="updateParentValue"
        name="agreeWithSendSms"
        label="Не отправлять СМС"
      />
    </div>
    <div class="address-form">
      <form-input
        label="Город"
        :is-required="true"
        name="city"
        @input="updateParentValue"
      />
      <form-input label="Индекс" name="postcode" @input="updateParentValue" />
      <form-input label="Страна" name="country" @input="updateParentValue" />
      <form-input label="Область" name="region" @input="updateParentValue" />
      <form-input label="Улица" name="street" @input="updateParentValue" />
      <form-input label="Дом" name="home" @input="updateParentValue" />
    </div>
    <div class="passport-form">
      <form-select
        :is-required="true"
        :options="documents"
        name="selectedDocument"
        label="Тип документа"
        @change="updateParentValue"
      />
      <form-input label="Серия" name="serial" @input="updateParentValue" />
      <form-input label="Номер" name="number" @input="updateParentValue" />
      <form-input
        label="Кем выдан"
        name="issuedBy"
        @input="updateParentValue"
      />
      <form-input
        label="Дата выдачи"
        :is-required="true"
        type="date"
        name="dateOfIssue"
        @input="updateParentValue"
      />
    </div>
  </form>
</template>

<script>
import FormInput from "@/components/UI/FormInput.vue";
import PhoneInput from "@/components/PhoneInput.vue";
import FormSelect from "@/components/UI/FormSelect.vue";
import FormMultipleSelect from "@/components/UI/FormMultipleSelect.vue";
import FormCheck from "@/components/UI/FormCheck.vue";
import RadioInput from "@/components/UI/RadioInput.vue";

export default {
  name: "App",
  components: {
    RadioInput,
    FormCheck,
    FormMultipleSelect,
    FormSelect,
    PhoneInput,
    FormInput,
  },
  data() {
    return {
      name: "",
      lastname: "",
      birthdate: "",
      phone: "",
      selectedGender: "",
      genderOption: [
        { label: "Мужчина", value: "Мужчина" },
        { label: "Женщина", value: "Женщина" },
      ],
      selectedGroups: [],
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
      selectedDoctor: null,
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
      agreeWithSendSms: false,
      city: "",
      postcode: "",
      country: "",
      region: "",
      street: "",
      home: "",
      serial: "",
      number: "",
      issuedBy: "",
      dateOfIssue: "",
      selectedDocument: "",
      documents: [
        {
          label: "Паспорт",
          value: "Паспорт",
        },
        {
          label: "Свидетельство о рождении",
          value: "Свидетельство о рождении",
        },
        {
          label: "Вод. удостоверение",
          value: "Вод. удостоверение",
        },
      ],
      successMsg: false,
    };
  },
  methods: {
    updateParentValue(name, newValue) {
      this[name] = newValue;
    },
  },
};
</script>

<style lang="scss">
.form {
  display: flex;
  align-items: flex-start;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 20px;
}

.main-form,
.address-form,
.passport-form {
  min-width: 300px;
  background: #eee;
  width: 30%;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  row-gap: 10px;
}

.main-form {
  &__row {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    column-gap: 20px;
    flex-wrap: wrap;
  }
}
</style>
