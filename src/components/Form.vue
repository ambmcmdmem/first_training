<template>
  <label style="display: block;" v-for="profileItem in profileItemDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" v-model="profileItems[profileItem.name]">
    <template v-if="validated">
      <span v-for="(error, errorNo) in errors[profileItem.name]" :key="profileItem.name + '-error-' + errorNo" style="color: red" v-cloak>
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
const dummyObj = {};
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
const profileItemDefinitions = [
  {
    ...commonForProfileItems.name,
    type: 'text',
  },
  {
    ...commonForProfileItems.email,
    type: 'email',
  },
  {
    ...commonForProfileItems.password,
    type: 'password',
  }    
];

export default {
  data() {
    return {
      profileItems: {
        name: '',
        email: '',
        password: ''
      },
      errors: {
        name: [],
        email: [],
        password: []
      },
      validated: false
    };
  },
  methods: {
    setProfile() {
      this.errors.name = validationErrors(
        [this.profileItems.name, '入力されていません。']
      );
      this.errors.email = validationErrors(
        [this.profileItems.email, '入力されていません。']
      );
      this.errors.password = validationErrors(
        [this.profileItems.password, '入力されていません。'],
        [this.profileItems.password.length >= 8, 'パスワードは8文字以上です。']
      );
      this.validated = true;
      this.$emit('setProfile', this.hasError ? dummyObj : [
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
      return Object.values(this.errors).some(error => error.length);
    },
    profileItemDefinitions() {
      return profileItemDefinitions;
    }
  }
};

</script>
