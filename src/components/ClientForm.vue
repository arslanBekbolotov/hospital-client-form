<template>
  <form class="form" @submit.prevent="checkResult">
    <div class="form-content">
      <div class="main-form">
        <form-input
          label="Имя"
          :is-required="true"
          name="name"
          @input="updateParentValue"
          :validation="$v"
        />
        <form-input
          label="Фамилия"
          :is-required="true"
          name="lastname"
          @input="updateParentValue"
          :validation="$v"
        />
        <form-input
          label="Дата рождения"
          :is-required="true"
          type="date"
          name="birthdate"
          @input="updateParentValue"
          :validation="$v"
        />
        <phone-input
          name="phone"
          label="Номер телефона"
          :is-required="true"
          :validation="$v"
          @blur="updateParentValue"
        />
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
          :validation="$v"
        />
        <form-select
          :options="doctors"
          name="selectedDoctor"
          label="Лечащий врач"
          @change="updateParentValue"
          :validation="$v"
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
          :validation="$v"
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
          :validation="$v"
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
          :validation="$v"
        />
      </div>
    </div>
    <form-button class="form-button" button-text="Создать">
      <img class="button__icon" src="@/assets/icons8-send-96.png" alt="send" />
    </form-button>
    <form-alert
      v-if="!$v.$error"
      :visible="visible"
      message="Пользователь успешно создан"
    />
    <form-alert
      v-if="$v.$error"
      :visible="visible"
      :error="true"
      message="Заполните все обязательные поля"
    />
  </form>
</template>

<script>
import FormInput from "@/components/UI/FormInput.vue";
import PhoneInput from "@/components/PhoneInput.vue";
import FormSelect from "@/components/UI/FormSelect.vue";
import FormMultipleSelect from "@/components/UI/FormMultipleSelect.vue";
import FormCheck from "@/components/UI/FormCheck.vue";
import RadioInput from "@/components/UI/RadioInput.vue";
import { required, minLength } from "vuelidate/lib/validators";
import FormButton from "@/components/UI/FormButton.vue";
import FormAlert from "@/components/FormAlert.vue";

export default {
  name: "App",
  components: {
    FormAlert,
    FormButton,
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
      visible: false,
    };
  },
  validations: {
    name: { required },
    lastname: { required },
    birthdate: { required },
    phone: {
      required,
      minLength: minLength(11),
      validPhone: (val) => /^7\d{10}$/.test(val),
    },
    selectedGroups: { required },
    city: { required },
    selectedDocument: { required },
    dateOfIssue: { required },
  },
  methods: {
    updateParentValue(name, newValue) {
      this[name] = newValue;
    },
    checkResult() {
      this.$v.$touch();
      this.visible = true;
      setTimeout(() => {
        this.visible = false;
      }, 5000);
    },
  },
};
</script>

<style lang="scss">
.form-content {
  display: flex;
  align-items: flex-start;
  justify-content: space-evenly;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
  position: relative;
}

.main-form,
.address-form,
.passport-form {
  min-width: 300px;
  background: #82b3f3;
  width: 30%;
  border: 1px solid #ccc;
  border-radius: 10px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  row-gap: 10px;
}

.form-button {
  position: absolute;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
}
</style>
