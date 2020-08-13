<template>
  <div class="container">
    <form @submit.prevent="checkForm">
      <h2>Пользователь</h2>
      <ul class="flex-outer">
        <li>
          <label for="second-name">
            Фамилия
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              :class="$v.form.secondName.$error ? 'invalid' : ''"
              v-model.trim="form.secondName"
              @blur="$v.form.secondName.$touch()"
              type="text"
              id="second-name"
              placeholder="Введите вашу фамилию"
            />
            <span
              v-if="$v.form.secondName.$error && !$v.form.secondName.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
          </span>
        </li>
        <li>
          <label for="first-name">
            Имя
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              v-model.trim="form.firstName"
              @blur="$v.form.firstName.$touch()"
              :class="$v.form.firstName.$error ? 'invalid' : '' "
              type="text"
              id="first-name"
              placeholder="Введите ваше имя"
            />
            <span
              v-if="$v.form.firstName.$error && !$v.form.firstName.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
          </span>
        </li>
        <li>
          <label for="last-name">Отчество</label>
          <input
            v-model.trim="form.lastName"
            type="text"
            id="last-name"
            placeholder="Введите свое отчество"
          />
        </li>
        <li>
          <label for="birthday">
            Дата рождения
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              @blur="$v.form.birthday.$touch()"
              v-model="form.birthday"
              :class="$v.form.birthday.$error ? 'invalid' : ''"
              type="date"
              id="birthday"
            />
            <span
              v-if="$v.form.birthday.$error && !$v.form.birthday.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
          </span>
        </li>
        <li>
          <label for="phone">
            Номер телефона
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              v-model="form.phoneNumber"
              @blur="$v.form.phoneNumber.$touch()"
              :class="$v.form.phoneNumber.$error ? 'invalid' : ''"
              type="tel"
              id="phone"
            />
            <span
              v-if="$v.form.phoneNumber.$error && !$v.form.phoneNumber.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
            <span
              v-if="$v.form.phoneNumber.$error && (!$v.form.phoneNumber.minLength || !$v.form.phoneNumber.maxLength) "
              class="validation-error"
            >Неккоректная длина номера: вводите данные в формате +79970080540</span>
          </span>
        </li>
        <li>
          <label>Пол</label>
          <div class="select-male">
            <span>
              <label for="male">Мужской</label>
              <input id="male" type="radio" value="male" name="genderRadio" v-model="form.gender" />
            </span>
            <span>
              <label for="female">Женский</label>
              <input
                id="female"
                type="radio"
                value="female"
                name="genderRadio"
                v-model="form.gender"
              />
            </span>
          </div>
        </li>
        <li title="Чтобы выбрать несколько групп клиентов зажмите CTRL">
          <label for="client-select">
            Группа клиентов
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <select
              id="client-select"
              v-model="form.selectedClients"
              multiple
            >
              <option
                v-for="(client, index) of clients"
                :value="client.value"
                :key="index"
              >{{client.label}}</option>
            </select>
            <span
              class="validation-error"
              v-if="$v.form.selectedClients && !$v.form.selectedClients.required"
            >Это поле не может быть пустым</span>
          </span>
        </li>
        <li>
          <label for="doctor-select">Лечащий врач</label>
          <select id="doctor-select">
            <option
              v-for="(doctor, index) of doctors"
              :value="doctor.value"
              :key="index"
            >{{ doctor.label }}</option>
          </select>
        </li>
        <li>
          <label for="send-sms">Не отправлять смс</label>
          <div>
            <input type="checkbox" v-model="form.agreeWithSendMessage" />
          </div>
        </li>
      </ul>
      <h2>Адрес</h2>
      <ul class="flex-outer">
        <li>
          <label for="index">Индекс</label>
          <span class="input-wrapper">
            <input v-model="form.index" type="text" id="index" placeholder="Введите индекс" />
          </span>
        </li>
        <li>
          <label for="country">Страна</label>
          <span class="input-wrapper">
            <input v-model="form.country" type="text" id="country" placeholder="Введите страну" />
          </span>
        </li>
        <li>
          <label for="region">Область</label>
          <span class="input-wrapper">
            <input v-model="form.region" type="text" id="region" placeholder="Введите область" />
          </span>
        </li>
        <li>
          <label for="city">
            Город
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              v-model.trim="form.city"
              @blur="$v.form.city.$touch()"
              :class="$v.form.city.$error ? 'invalid' : ''"
              v-model="form.city"
              placeholder="Введите город"
              type="text"
              id="city"
            />
            <span
              v-if="$v.form.city.$error && !$v.form.city.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
          </span>
        </li>
        <li>
          <label for="street">Улица</label>
          <span class="input-wrapper">
            <input placeholder="Введите улицу" v-model="form.street" type="text" id="street" />
          </span>
        </li>
        <li>
          <label for="house">Дом</label>
          <span class="input-wrapper">
            <input v-model="form.house" type="text" id="house" placeholder="Введите дом" />
          </span>
        </li>
      </ul>
      <h2>Документы</h2>
      <ul class="flex-outer">
        <li>
          <label for="document-select">
            Тип документа
            <span class="required">*</span>
          </label>
          <select
            :class="$v.form.selectedDocuments.$error ? 'invalid' : ''"
            id="document-select"
            v-model="form.selectedDocuments"
          >
            <option
              v-for="(document, index) of documents"
              :value="document.value"
              :key="index"
            >{{document.label}}</option>
          </select>
          <span
            v-if="$v.form.selectedDocuments.$error && !$v.form.selectedDocuments.required"
            class="validation-error"
          >Это поле не может быть пустым</span>
        </li>
        <li>
          <label for="passport-series">Серия</label>
          <span class="input-wrapper">
            <input
              v-model.trim="form.passportSeries"
              type="text"
              id="passport-series"
              placeholder="Укажите серию паспорта"
            />
          </span>
        </li>
        <li>
          <label for="passport-number">Номер</label>
          <span class="input-wrapper">
            <input
              v-model.trim="form.passportNumber"
              type="text"
              id="passport-number"
              placeholder="Укажите номер паспорта"
            />
          </span>
        </li>
        <li>
          <label for="issued-by">Кем выдан</label>
          <span class="input-wrapper">
            <input
              v-model.trim="form.passportIssuedBy"
              type="text"
              id="issued-by"
              placeholder="Паспорт выдан"
            />
          </span>
        </li>
        <li>
          <label for="passportDate">
            Дата выдачи
            <span class="required">*</span>
          </label>
          <span class="input-wrapper">
            <input
              @blur="$v.form.passportDate.$touch()"
              :class="$v.form.passportDate.$error ? 'invalid' : ''"
              v-model="form.passportDate"
              type="date"
              id="passportDate"
            />
            <span
              v-if="$v.form.passportDate.$error && !$v.form.passportDate.required"
              class="validation-error"
            >Это поле не может быть пустым</span>
          </span>
        </li>
      </ul>
      <button :disabled="$v.$invalid" type="submit" class="create-client">Создать клиента</button>
    </form>
    <div v-if="form.isValidForm && !form.close" class="successful-create">
      <div>
        <p>Клиент успешно создан</p>
      </div>
      <div>
        <p class="close" @click="form.close = !form.close">&times;</p>
      </div>
    </div>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, minLength, maxLength } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],
  data() {
    return {
      form: {
        secondName: "",
        firstName: "",
        lastName: "",
        birthday: "",
        index: "",
        country: "",
        region: "",
        city: "",
        street: "",
        house: "",
        passportSeries: "",
        passportNumber: "",
        passportIssuedBy: "",
        passportDate: "",
        phoneNumber: "+7",
        gender: "male",
        client: "VIP",
        selectedClients: ["VIP"],
        selectedDocuments: "passport",
        agreeWithSendMessage: false,
        isValidForm: false,
        close: true,
      },
      clients: [
        {
          label: "VIP",
          value: "VIP",
        },
        {
          label: "Проблемные",
          value: "Problematic",
        },
        {
          label: "ОМС",
          value: "OMS",
        },
      ],
      doctors: [
        {
          label: "Не выбрано",
          value: "Empty"
        },
        {
          label: "Иванов",
          value: "Ivanov",
        },
        {
          label: "Захаров",
          value: "Zakharov",
        },
        {
          label: "Чернышева",
          value: "Chernysheva",
        },
      ],
      documents: [
        { label: "Паспорт", value: "passport" },
        { label: "Свидетельство о рождении", value: "birth-certificate" },
        { label: "Водительское удостов.", value: "drivers-license" },
      ],
    };
  },
  validations: {
    form: {
      secondName: { required },
      firstName: { required },
      birthday: { required },
      phoneNumber: {
        required,
        minLength: minLength(12),
        maxLength: maxLength(12),
      },
      city: { required },
      selectedDocuments: { required },
      selectedClients: { required },
      passportDate: { required },
    },
  },
  methods: {
    checkForm() {
      this.$v.form.$touch();
      if (!this.$v.form.$error) {
        this.form.isValidForm = true;
        this.form.close = false;
      } else {
        this.form.isValidForm = false;
      }
    },
  },
};
</script>


<style lang="sass">
body
  font: normal 18px/1.5 "Fira Sans", "Helvetica Neue", sans-serif
  background: #EDEDED
  color: #949494
  padding: 50px 0

h2 
  text-align: center
  color: #48484A

.successful-create 
  margin-top: 20px;
  padding: 10px 20px;
  border-radius: 7px;
  line-height: 30px
  width: 100%;
  color: #fff;
  font-size: 20px;
  background-color: #3FB37F;
  display: flex
  justify-content: space-between
  align-items: center

.successful-create .close 
  font-size: 30px
  line-height: 20px
  cursor: pointer

.successful-create .close:hover 
  color: #EDECF3
  transition: all .15s linear

.input-wrapper
  display: flex
  flex-direction: column

.required 
  color: red

.validation-error
  font-size: 14px
  color: #E32636

form input.invalid, form select.invalid
  border: 2px solid #E32636
  transition: border .1s linear

.container 
  width: 100%
  max-width: 1200px
  margin: 0 auto

.container * 
  box-sizing: border-box
select 
  box-sizing: border-box


.flex-outer,
.flex-inner 
  list-style-type: none
  padding: 0


.flex-outer 
  max-width: 800px
  margin: 0 auto


.flex-outer li,
.flex-inner 
  display: flex
  flex-wrap: wrap
  justify-content: space-between
  align-items: center
  width: 100%

.flex-inner 
  padding: 0 8px


.flex-outer > li:not(:last-child) 
  margin-bottom: 20px


.flex-outer li label,
.flex-outer li p 
  padding: 8px
  font-weight: 300
  letter-spacing: .09em

.flex-outer > li > label,
.flex-outer li p 
  flex: 1 0 120px
  max-width: 220px


.flex-outer > li > label + *,
.flex-inner 
  flex: 1 0 220px


.flex-outer li p 
  margin: 0


input:not([type="checkbox"]),
li, select
  padding: 15px
  border: none
  outline: none


.flex-outer li button 
  margin-left: auto
  padding: 8px 16px
  border: none
  background: #333
  color: #f2f2f2
  text-transform: uppercase
  letter-spacing: .09em
  border-radius: 2px

.flex-inner li 
  width: 100px

textarea
  resize: none

form 
  background: #ffffff
  padding: 20px
  border-radius: 7px
  width: 100%
  max-width: 1200px

input, textarea, select
  background: #EDEDED

select option 
  padding: 10px
  word-wrap: break-word

.create-client
  width: 100%
  margin: 10px 0px
  border: none
  padding: 15px
  color: #ffffff
  background: #3467BA
  font-size: 20px
  outline: none
  border-radius: 5px
  cursor: pointer

.create-client:hover
  background: #205BBA
  transition: all 0.3s linear

.create-client:disabled
  background: #C3C3C3
  color: #7A7A7A
  cursor: not-allowed


</style>
