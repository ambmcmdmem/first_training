<template>
  <label style="display: block;" v-for="profileItem in profileItemDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" v-model="profileItems[profileItem.name]">
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
const commonForProfileItems = {
  name: {
    name: 'name'
  },
  email: {
    name: 'email'
  },
  password: {
    name: 'password'
  }
};

export default {
  data() {
    return {
      profileItems: {
        name: '',
        email: '',
        password: ''
      },
      profileItemErrors: {
        name: [],
        email: [],
        password: []
      },
      validate: false
    };
  },
  methods: {
    setProfile() {
      this.profileItemErrors.name = validationErrors(
        [this.profileItems.name, '入力されていません。']
      );
      this.profileItemErrors.email = validationErrors(
        [this.profileItems.email, '入力されていません。']
      );
      this.profileItemErrors.password = validationErrors(
        [this.profileItems.password, '入力されていません。'],
        [this.profileItems.password.length >= 8, 'パスワードは8文字以上です。']
      );
      this.validate = true;
      this.$emit('setProfile', this.hasError ? {} : [
        {
          ...commonForProfileItems.name,
          input: this.profileItems.name
        },
        {
          ...commonForProfileItems.email,
          input: this.profileItems.email
        },
        {
          ...commonForProfileItems.password,
          input: this.profileItems.password
        }
      ]);
    }
  },
  computed: {
    hasError() {
      return this.profileItemDefinitions
        .some(profileItem => profileItem.errors.length);
    },
    profileItemDefinitions() {
      return [
        {
          ...commonForProfileItems.name,
          type: 'text',
          errors: this.profileItemErrors.name
        },
        {
          ...commonForProfileItems.email,
          type: 'email',
          errors: this.profileItemErrors.email
        },
        {
          ...commonForProfileItems.password,
          type: 'password',
          errors: this.profileItemErrors.password
        }    
      ];
    }
  }
};

</script>
