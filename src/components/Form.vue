<template>
  <label style="display: block;" v-for="profileItem in profileItemDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" :ref="profileItem.name">
    <template v-if="validate">
      <span v-for="(error, errorNo) in profileItem.errors" :key="profileItem.name + '-error-' + errorNo" style="color: red" v-cloak>
        {{ error }}
      </span>
    </template>
  </label>
  <button type="button" @click="setProfile">SUBMIT</button>
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
      validate: false
    };
  },
  methods: {
    setProfile() {
      this.name = this.$refs.name[0].value;
      this.email = this.$refs.email[0].value;
      this.password = this.$refs.password[0].value;
      this.validate = true;
      this.$emit('setProfile', this.hasError ? {} : this.profileItems);
    }
  },
  computed: {
    profileItems() {
      return [
        {
          name: 'name',
          input: this.name
        },
        {
          name: 'email',
          input: this.email
        },
        {
          name: 'password',
          input: this.password
        }
      ];
    },
    hasError() {
      return this.profileItemDefinitions
        .some(profileItem => profileItem.errors.length);
    },
    profileItemDefinitions() {
      return [
        {
          name: 'name',
          type: 'text',
          errors: validationErrors(
            [this.name, '入力されていません。']
          ),
        },
        {
          name: 'email',
          type: 'email',
          errors: validationErrors(
            [this.email, '入力されていません。']
          ),
        },
        {
          name: 'password',
          type: 'password',
          errors: validationErrors(
            [this.password, '入力されていません。'],
            [this.password.length >= 8, 'パスワードは8文字以上です。']
          ),
        }    
      ];
    }
  }
};

</script>
