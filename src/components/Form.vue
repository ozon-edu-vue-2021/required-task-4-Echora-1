<template>
  <div class="form">
    <div class="row">

      <div class="w-100 title">
        <p>Личные данные</p>
      </div>
      <div class="col w-33">
        <BaseInput label="Фамилия" id="Surname" v-model="form.surname"/>
      </div>
      <div class="col w-33">
        <BaseInput label="Имя" id="Name" v-model="form.name"/>
      </div>
      <div class="col w-33">
        <BaseInput label="Очество" id="middleName" v-model="form.middleName"/>
      </div>
      <div class="col w-50">
        <BaseInput placeholder="дд.мм.гггг" label="Дата рождения" id="dateBirth" v-model="form.dateBirth"/>
      </div>
      <div class="col w-100">
        <BaseInput label="Email" id="email" v-model="form.email"/>
      </div>

      <div class="w-100 title">
        <p>Пол</p>
      </div>
      <div class="col w-20">
        <BaseRadioButton label="Мужской" value="male" v-model="form.gender" />
      </div>
      <div class="col w-20">
        <BaseRadioButton label="Женский" value="female" v-model="form.gender" />
      </div>

      <div class="w-100 title">
        <p>Паспортные данные</p>
      </div>
      <div class="col w-50">
        <BaseDropdown :list="listCitizenships" item-key="nationality" label="Гражданство" id="сitizenship" @selected="(value) => selectCitizenship(value)"/>
      </div>
      <div class="w-100 d-f" v-if="isRussianCitizenship">
        <div class="col w-25">
          <BaseInput label="Серия паспорта" id="passportSeries" v-model="form.passportSeries"/>
        </div>
        <div class="col w-25">
          <BaseInput label="Номер паспорта" id="passportID" v-model="form.passportID"/>
        </div>
        <div class="col w-50">
          <BaseInput placeholder="дд.мм.гггг" label="Дата выдачи" id="dateIssue" v-model="form.dateIssue"/>
        </div>
      </div>
      <div class="w-100 d-f" v-else>
        <div class="col w-50">
          <BaseInput label="Фамилия на латинице" id="surnameLatin" v-model="form.surnameLatin"/>
        </div>
        <div class="col w-50">
          <BaseInput label="Имя на латинице" id="nameLatin" v-model="form.nameLatin"/>
        </div>
        <div class="col w-25">
          <BaseInput label="Номер паспорта" id="passportID" v-model="form.passportID"/>
        </div>
        <div class="col w-25">
          <BaseDropdown :list="listCitizenships" item-key="nationality" label="Страна выдачи" id="сountryIssue" @selected="(value) => selectCountryIssue(value)"/>
        </div>
        <div class="col w-50">
          <BaseDropdown :list="listPassportTypes" item-key="type" label="Тип паспорта" id="passportType" @selected="(value) => selectPassportType(value)"/>
        </div>
      </div>
      <div class="w-100 title">
        <p>Меняли ли фамилию или имя?</p>
      </div>
      <div class="col w-20">
        <BaseRadioButton label="Нет" value="false" v-model="changeInitials" />
      </div>
      <div class="col w-20">
        <BaseRadioButton label="Да" value="true" v-model="changeInitials" />
      </div>
      <div class="w-100 d-f" v-if="isChangeInitials">
        <div class="col w-50">
          <BaseInput label="Предыдущая Фамилия" id="previousSurname" v-model="form.previousSurname"/>
        </div>
        <div class="col w-50">
          <BaseInput label="Предыдущее Имя" id="previousName" v-model="form.previousName"/>
        </div>
      </div>

      <button class="submit" @click="submit">Отправить</button>
    </div>
  </div>
</template>

<script>

import BaseInput from "./BaseInput";
import BaseRadioButton from "./BaseRadioButton";
import BaseDropdown from "./BaseDropdown";
import citizenships from  "../assets/data/citizenships.json";
import passportTypes from  "../assets/data/passport-types.json";

export default {
  components: {BaseDropdown, BaseRadioButton, BaseInput},
  data() {
    return {
      changeInitials: 'false',
      listCitizenships: citizenships,
      listPassportTypes: passportTypes,
      form: {
        surname: '',
        name: '',
        middleName: '',
        dateBirth: '',
        email: '',
        gender: 'male',
        citizenships: '',
        surnameLatin: '',
        nameLatin: '',
        passportID: '',
        countryIssue: '',
        passportType: '',
        passportSeries: '',
        dateIssue: '',
        previousSurname: '',
        previousName: ''
      }
    };
  },

  methods: {
    selectCitizenship(citizenship) {
      this.form.citizenships = citizenship.nationality
    },

    selectCountryIssue(citizenship) {
      this.form.countryIssue = citizenship.nationality
    },

    selectPassportType(passportType) {
      this.form.passportType = passportType.type
    },

    reset() {
      this.form.surname = ''
      this.form.name = ''
      this.form.middleName = ''
      this.form.dateBirth = ''
      this.form.email = ''
      this.form.gender = 'male'
      this.form.citizenships = {}
      this.form.surnameLatin = ''
      this.form.nameLatin = ''
      this.form.passportID = ''
      this.form.countryIssue = ''
      this.form.passportType = ''
      this.form.passportSeries = ''
      this.form.dateIssue = ''
      this.form.previousSurname = ''
      this.form.previousName = ''
      this.changeInitials = 'false'
    },

    submit() {
      console.log(JSON.stringify(this.form))
      this.reset()
    }

  },

  computed: {
    isRussianCitizenship() {
      return this.form.citizenships === 'Russia' || this.form.citizenships === ''
    },

    isChangeInitials() {
      return this.changeInitials === 'true'
    }

  },

  watch: {
    isRussianCitizenship() {
      this.form.surnameLatin = ''
      this.form.nameLatin = ''
      this.form.passportID = ''
      this.form.countryIssue = ''
      this.form.passportType = ''
      this.form.passportSeries = ''
      this.form.dateIssue = ''
    }
  }
};
</script>

<style scoped>
.form {
  display: flex;
}

.title {
  font-size: 24px;
  padding: 0 20px;
}

.title p {
  margin: 12px 0;
}

.row {
  display: flex;
  flex-wrap: wrap;
  width: 800px;
  margin: 0 auto;
}

.col {
  padding: 20px;
}

.w-20 {
  flex: 0 0 20%;
  max-width: 20%;
}

.w-25 {
  flex: 0 0 25%;
  max-width: 25%;
}

.w-33 {
  flex: 0 0 33.3%;
  max-width: 33.3%;
}

.w-50 {
  flex: 0 0 50%;
  max-width: 50%;
}

.w-100 {
  flex: 0 0 100%;
  max-width: 100%;
}

.d-f {
  display: flex;
  flex-wrap: wrap;
}

.submit {
  width: 120px;
  height: 40px;
  background-color: #265BF6;
  border: none;
  color: white;
  border-radius: 4px;
  margin: 100px 20px 30px auto;
  cursor: pointer;
}
</style>
