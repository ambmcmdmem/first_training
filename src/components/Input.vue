<template>
  <label style="display: block;" v-for="profileItem in profileDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" v-model="profile[profileItem.name]">
    <span v-for="(error, errorNo) in errors[profileItem.name]" :key="profileItem.name + '-error-' + errorNo" style="color: red" v-cloak>
      {{ error }}
    </span>
  </label>
  <button type="button" @click="setProfile">SUBMIT</button>
</template>

<script>
import {emitter} from '../emitter';

const byExistence = (target) => target;
const validationErrors = (...validityAndErrors) =>
  validityAndErrors
    .map(([isValid, error]) => isValid ? '' : error)
    .filter(byExistence);
const blankProfile = {};
const profileDefinitions = [
  {
    name: 'name',
    type: 'text'
  },
  {
    name: 'email',
    type: 'email'
  },
  {
    name: 'password',
    type: 'password'
  }    
];

export default {
  data() {
    return {
      profile: {
        name: '',
        email: '',
        password: ''
      },
      errors: {
        name: [],
        email: [],
        password: []
      }
    };
  },
  methods: {
    validate() {
      this.errors = {
        name: validationErrors(
          [this.profile.name, '入力されていません。']
        ),
        email: validationErrors(
          [this.profile.email, '入力されていません。']
        ),
        password: validationErrors(
          [this.profile.password, '入力されていません。'],
          [this.profile.password.length >= 8, 'パスワードは8文字以上です。']
        )
      };
    },
    setProfile() {
      this.validate();

      emitter.emit('setProfile', this.hasError ? blankProfile : JSON.parse(JSON.stringify(this.profile)));
    }
  },
  computed: {
    hasError() {
      return Object.values(this.errors).some(error => error.length);
    },
    profileDefinitions() {
      return profileDefinitions;
    }
  }
};

</script>
