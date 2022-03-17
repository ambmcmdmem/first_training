<template>
  <label style="display: block;" v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
    {{ profileItemLabel }}:
    <input :type="profileItemLabel" :ref="profileItemLabel">
    <template v-if="validate">
      <span
        v-for="(errorMessage, errorMessageNo) in profileItem.errorMessages"
        :key="profileItemLabel + '-error-' + errorMessageNo"
        style="color: red"
        v-cloak
      >
        {{ errorMessage }}
      </span>
    </template>
  </label>
  <button @click="setProfile" type="button">SUBMIT</button>
</template>

<script>
export default {
  props: ['profileItems', 'validate'],
  data() {
    return {
      validate: false
    }
  },
  methods: {
    setProfile() {
      this.$emit(
        'setProfile', {
        name: {
          input: this.DoesProfilePassCheck(this.$refs) ? this.$refs.name[0].value : '',
          errorMessages: this.nameErrors(this.$refs.name[0].value)
        },
        email: {
          input: this.DoesProfilePassCheck(this.$refs) ? this.$refs.email[0].value : '',
          errorMessages: this.emailErrors(this.$refs.email[0].value)
        },
        password: {
          input: this.DoesProfilePassCheck(this.$refs) ? this.$refs.password[0].value : '',
          errorMessages: this.passwordErrors(this.$refs.password[0].value)
        }
      });
      this.validate = true;
    },
    DoesProfilePassCheck: function(refs) {
      if(this.nameErrors(refs.name[0].value).length) return false;
      if(this.emailErrors(refs.email[0].value).length) return false;
      if(this.passwordErrors(refs.password[0].value).length) return false;

      return true;
    },
    nameErrors: function(name) {
      console.log('name');
      const errors = [];
      if(!name) errors.push('入力されていません。');

      return errors;
    },
    emailErrors: function(email) {
      console.log('email');
      const errors = [];
      if(!email) errors.push('入力されていません。');

      return errors;
    },
    passwordErrors: function(password) {
      console.log('pass');
      const errors = [];
      if(!password) errors.push('入力されていません。');
      if(password.length < 8) errors.push('パスワードは8文字以上です。');

      return errors;
    }
  },
  
};
</script>