<template>
  <label style="display: block;" v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
    {{ profileItemLabel }}:
    <input :type="profileItemLabel" :ref="profileItemLabel">
    <template v-if="validate">
      <template v-for="(error, errorNo) in errors[profileItemLabel]" :key="profileItemLabel + '-error-' + errorNo">
        <span v-if="error" style="color: red" v-cloak>
          {{ error }}
        </span>
      </template>
    </template>
  </label>
  <button @click="setProfile" type="button">SUBMIT</button>
</template>

<script>
export default {
  props: ['profileItems', 'validate'],
  data() {
    return {
      errors: {},
      validate: false
    }
  },
  methods: {
    setProfile() {
      this.validate = true;

      this.$emit(
        'setProfile', {
        name: this.doExistErrors ? '' : this.$refs.name[0].value,
        email: this.doExistErrors ? '' : this.$refs.email[0].value,
        password: this.doExistErrors ? '' : this.$refs.password[0].value
      });
    },
  },
  computed: {
    doExistErrors: function() {
      return Object.values(this.errors)
        .some(error => error.length);
    },
    setErrors: function() {
      this.errors = {name: [], email: [], password: []};
      if(!this.profileItems.name) this.errors.name.push('入力されていません。');
      if(!this.profileItems.email) this.errors.email.push('入力されていません。');
      if(!this.profileItems.password) this.errors.password.push('入力されていません。');
      if(this.profileItems.password.length < 8) this.errors.password.push('パスワードは8文字以上です。');
    }
  }
};
</script>