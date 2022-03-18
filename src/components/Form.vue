<template>
  <form @submit.prevent="setProfile($event)">
    <label style="display: block;" v-for="(_, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
      {{ profileItemLabel }}:
      <input :type="profileTypes[profileItemLabel]" :name="profileItemLabel">
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
const validationErrors = (...validityAndErrors) =>
  validityAndErrors
    .map(([isValid, error]) => isValid ? '' : error)
    .filter(byExistence);

export default {
  data() {
    return {
      name: '',
      email: '',
      password: '',
      validate: false,
      profileTypes: {
        name: 'text',
        email: 'email',
        password: 'password'
      }
    };
  },
  methods: {
    setProfile($event) {
      this.name = $event.target.name.value
      this.email = $event.target.email.value
      this.password = $event.target.password.value
      this.validate = true;
      this.$emit('setProfile', this.doExistErrors ? {} : this.profileItems);
    }
  },
  computed: {
    profileItems() {
      return {
        name: this.name,
        email: this.email,
        password: this.password,
      };
    },
    doExistErrors() {
      return Object.values(this.errors)
        .some(error => error.length);
    },
    errors() {
      return {
        name: validationErrors(
          [this.name, '入力されていません。']
        ),
        email: validationErrors(
          [this.email, '入力されていません。']
        ),
        password: validationErrors(
          [this.password, '入力されていません。'],
          [this.password.length >= 8, 'パスワードは8文字以上です。']
        )        
      };
    }
  }
};
</script>
