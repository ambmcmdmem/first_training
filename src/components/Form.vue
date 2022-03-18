<template>
  <form @submit.prevent="setProfile($event)">
    <label style="display: block;" v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
      {{ profileItemLabel }}:
      <input :type="profileItem.type" :name="profileItemLabel">
      <template v-if="validate">
        <template v-for="(error, errorNo) in errors[profileItemLabel]" :key="profileItemLabel + '-error-' + errorNo">
          <span v-if="error" style="color: red" v-cloak>
            {{ error }}
          </span>
        </template>
      </template>
    </label>
    <button type="submit">SUBMIT</button>
  </form>
</template>

<script>
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
        input: this.name,
        type: 'text'
      };
    },
    emailInfo() {
      return {
        input: this.email,
        type: 'email'
      };
    },
    passwordInfo() {
      return {
        input: this.password.replace(/\S/g, '●'),
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
      const errors = {
        name: [],
        email: [],
        password: []
      };
      if(!this.name) errors.name.push('入力されていません。');
      if(!this.email) errors.email.push('入力されていません。');
      if(!this.password) errors.password.push('入力されていません。');
      if(this.password.length < 8) errors.password.push('パスワードは8文字以上です。');

      return errors;
    }
  }
};
</script>
