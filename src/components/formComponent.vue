<template>
  <form class="data-forms" @submit.prevent="submit">
    <div class="form-group" >
      <h2>Основные данные</h2>
      <label class="form__label">Фамилия*</label>
      <input class="form__input" type="text" 
                                 :class="status($v.form.surname)" 
                                 v-model.trim="$v.form.surname.$model"/>
      <div class="error" v-if="!$v.form.surname.required">Обязательное поле</div>
      <label class="form__label">Имя*</label>
      <input class="form__input" type="text" 
                                 :class="status($v.form.name)" 
                                 v-model.trim="$v.form.name.$model"/>
      <div class="error" v-if="!$v.form.name.required">Обязательное поле</div>
      <label class="form__label">Отчество</label>
      <input class="form__input" type="text" v-model.trim="$v.form.patronymic"/>
      <label class="form__label">Дата рождения*</label>
      <input class="form__input" type="date" 
                                 :class="status($v.form.dateBirth)" 
                                 v-model.trim="$v.form.dateBirth.$model"/>
      <div class="error" v-if="!$v.form.dateBirth.required">Обязательное поле</div>
      <label class="form__label">Номер телефона*</label>
      <input class="form__input" type="tel" 
                                 :class="status($v.form.tel)" 
                                 v-model.trim="$v.form.tel.$model"/> 
      <div class="error" v-if="!$v.form.tel.required">Обязательное поле</div>
      <div class="error" v-if="!$v.form.tel.maxLength">Длина 11 цифр</div>
      <label class="form__label">Пол</label>
      <label class="form__label"><input class="form__input" type="checkbox" id="gender" value="male" v-model.trim="$v.form.gender"/>М</label>
      <label class="form__label"><input class="form__input" type="checkbox" id="gender" value="female" v-model.trim="$v.form.gender"/>Ж</label>
      <label class="form__label">Группа клиентов*</label>
      <select class="form__input" multiple 
                                 :class="status($v.form.clientGroup)" 
                                 v-model="$v.form.clientGroup.$model">
        <option v-for="item in form.clientGroupOptions" :key="item.value" :value="item.value">
          {{item.text}}
        </option>
      </select>
      <div class="error" v-if="!$v.form.clientGroup.required">Обязательное поле</div>
      <label class="form__label">Лечащий врач</label>
      <select class="form__input" v-model="$v.form.doctor">
        <option v-for="item in form.doctorOptions" :key="item.value" :value="item.value">
          {{item.text}}
        </option>
      </select>
      <label class="form__label"><input class="form__input" type="checkbox" v-model.trim="$v.form.sendSms"/>Не отправлять СМС</label>
    </div>

    <div class="form-group" >
      <h2>Адрес</h2>
      <label class="form__label">Индекс</label>
      <input class="form__input" type="number" v-model.trim="$v.form.countryIndex"/>
      <label class="form__label">Страна</label>
      <input class="form__input" type="text" v-model.trim="$v.form.country"/>
      <label class="form__label">Область</label>
      <input class="form__input" type="text" v-model.trim="$v.form.region"/>
      <label class="form__label">Город*</label>
      <input class="form__input" type="text" 
                                 :class="status($v.form.town)" 
                                 v-model.trim="$v.form.town.$model"/>
      <div class="error" v-if="!$v.form.town.required">Обязательное поле</div>
      <label class="form__label">Улица</label>
      <input class="form__input" type="text" v-model.trim="$v.form.street"/>
      <label class="form__label">Дом</label>
      <input class="form__input" type="text" v-model.trim="$v.form.house"/>
    </div>

    <div class="form-group">
      <h2>Паспорт</h2>
      <label class="form__label">Тип документа*</label>
      <select class="form__input" 
                                 :class="status($v.form.docType)" 
                                 v-model="$v.form.docType.$model" >
        <option v-for="item in form.docTypeOptions" :value="item.value" :key="item.value">
          {{item.text}}
        </option>
      </select>
      <div class="error" v-if="!$v.form.docType.required">Обязательное поле</div>
      <label class="form__label">Серия</label><input class="form__input" type="number" v-model.trim="$v.form.docSeries"/>
      <label class="form__label">Номер</label><input class="form__input" type="number" v-model.trim="$v.form.docNumber"/>
      <label class="form__label">Кем выдан</label>
      <input class="form__input" type="text" v-model.trim="$v.form.docIssuedBy"/>
      <label class="form__label">Дата выдачи*</label>
      <input class="form__input" type="date" 
                                 :class="status($v.form.docDate)" 
                                 v-model.trim="$v.form.docDate.$model"/>
      <div class="error" v-if="!$v.form.docDate.required">Обязательное поле</div>
      <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">Создать</button>
      <p class="button__sub" v-if="submitStatus === 'OK'">Клиент создан</p>
      <p class="button__sub" v-if="submitStatus === 'ERROR'">Не все поля заполнены верно</p>
      <p class="button__sub" v-if="submitStatus === 'PENDING'">Создание</p>
    </div>
    </form>
</template>

<script>
import { required, minLength, maxLength } from 'vuelidate/lib/validators'

export default {
  name: 'FormComponent',
  data() {
    return{
      submitStatus: null,
      form: {
        surname: '',
        name:'',
        patronymic: '',
        dateBirth: '',
        tel: '+7',
        gender: false,
        clientGroup: [],
        clientGroupOptions: [
          {value: 'client1', text: 'VIP'}, 
          {value: 'client2', text: 'Проблемные'}, 
          {value: 'client3', text: 'ОМС'}
        ],
        doctor: 'doc1',
        doctorOptions: [
          {value: 'doc1', text: 'Иванов'}, 
          {value: 'doc2', text: 'Захаров'}, 
          {value: 'doc3', text: 'Чернышева'}
        ],
        sendSms: false,
        countryIndex: '',
        country: '',
        region: '',
        town: '',
        street: '',
        house: '',
        docType: 'docT1',
        docTypeOptions: [
          {value: 'docT1', text: 'Паспорт'}, 
          {value: 'docT2', text: 'Свидетельство о рождении'}, 
          {value: 'docT3', text: 'Вод. удостоверение'}
        ],
        docSeries: '',
        docNumber: '',
        docIssuedBy: '',
        docDate: '',
      }
    }
  },
  validations: {
    form: {
      surname: {required},
      name: {required},
      dateBirth: {required},
      tel: {required, minLength: minLength(11), maxLength: maxLength(11)},
      clientGroup: {required},
      town: {required},
      docType: {required},
      docDate: {required},
    }
  },
  methods: {
    status(validation) {
    	return {
      	error: validation.$error,
        dirty: validation.$dirty
      }
    },
    submit() {
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.submitStatus = 'PENDING'
        setTimeout(()=>{
          this.submitStatus = 'OK'
        }, 500)
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="sass">
.data-forms
  display: flex
  flex-direction: row
  flex-wrap: wrap
.form-group
  display: flex
  flex-direction: column
  align-items: flex-start
  flex: auto
  min-width: 250px
  margin: 0 10px
  input, select
    width: 100%
    margin-bottom: 10px
    padding: 5px 0 5px 5px
    border: 1px solid gray
    border-radius: 4px
    outline: none
.dirty
  &:focus
    border-color: green
.error
  &:focus
    border-color: red
</style>
