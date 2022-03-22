<template>
  <label style="display: block;" v-for="profileItem in profileItemDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" v-model="profileItems[profileItem.name]">
    <template v-if="isInitialDisplay">
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
const dummyProfileItemsToExport = {};
const profileItemDefinitions = [
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
      isInitialDisplay: false
    };
  },
  methods: {
    setErrors() {
      this.errors = {
        name: validationErrors(
          [this.profileItems.name, '入力されていません。']
        ),
        email: validationErrors(
          [this.profileItems.email, '入力されていません。']
        ),
        password: validationErrors(
          [this.profileItems.password, '入力されていません。'],
          [this.profileItems.password.length >= 8, 'パスワードは8文字以上です。']
        )
      };
    },
    setProfile() {
      this.setErrors();

      this.isInitialDisplay = true;
      this.$emit('setProfile', this.hasError ? dummyProfileItemsToExport : {
        name: this.profileItems.name,
        email: this.profileItems.email,
        password: this.profileItems.password
      });
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
