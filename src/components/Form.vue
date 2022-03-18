<template>
  <form @submit.prevent="setProfile($event)">
    <label style="display: block;" v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
      {{ profileItemLabel }}:
      <input :type="profileItem.type" :name="profileItemLabel">
      <template v-if="validate">
        <span v-for="(error, errorNo) in errors[profileItemLabel]" :key="profileItemLabel + '-error-' + errorNo" style="color: red" v-cloak>
          {{ error }}
        </span>
      </template>
    </label>
    <button type="submit">SUBMIT</button>
  </form>
</template>

<script>
const byExistence = (target) => target;
const validateErrors = (...validationAndErrors) =>
  validationAndErrors
    .map(([isValid, error]) => isValid ? '' : error)
    .filter(byExistence);

export default {
  data() {
    return {
      name: '',
      email: '',
      password: '',
      validate: false
    };
  },
  methods: {
    setProfile($event) {
      this.name = $event.target.name.value
      this.email = $event.target.email.value
      this.password = $event.target.password.value
      this.validate = true;
      this.$emit('setProfile', this.profileItems);
    }
  },
  computed: {
    nameInfo() {
      return {
        input: this.doExistErrors ? '' : this.name,
        type: 'text'
      };
    },
    emailInfo() {
      return {
        input: this.doExistErrors ? '' : this.email,
        type: 'email'
      };
    },
    passwordInfo() {
      return {
        input: this.doExistErrors ? '' : this.password.replace(/\S/g, '●'),
        type: 'password'
      };
    },
    profileItems() {
      return {
        name: this.nameInfo,
        email: this.emailInfo,
        password: this.passwordInfo
      };
    },
    doExistErrors() {
      return Object.values(this.errors)
        .some(error => error.length);
    },
    errors() {
      return {
        name: validateErrors(
          [this.name, '入力されていません。']
        ),
        email: validateErrors(
          [this.email, '入力されていません。']
        ),
        password: validateErrors(
          [this.password, '入力されていません。'],
          [this.password.length >= 8, 'パスワードは8文字以上です。']
        )        
      }
    }
  }
};
</script>
