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
      errors: {},
      validate: false
    };
  },
  methods: {
    setProfile($event) {
      this.validate = true;
      this.errors = {name: [], email: [], password: []};
      if(!$event.target.name.value) this.errors.name.push('入力されていません。');
      if(!$event.target.email.value) this.errors.email.push('入力されていません。');
      if(!$event.target.password.value) this.errors.password.push('入力されていません。');
      if($event.target.password.value.length < 8) this.errors.password.push('パスワードは8文字以上です。');
      this.name = this.doExistErrors ? '' : $event.target.name.value;
      this.email = this.doExistErrors ? '' : $event.target.email.value;
      this.password = this.doExistErrors ? '' : $event.target.password.value;

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
    }
  }
};
</script>
