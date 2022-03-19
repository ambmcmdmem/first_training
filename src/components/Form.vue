<template>
  <label style="display: block;" v-for="profileItem in profileItemDefinitions" :key="profileItem.name + '-input'">
    {{ profileItem.name }}:
    <input :type="profileItem.type" v-model="profileItemsData[profileItem.name]">
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
      profileItemsData: {
        name: '',
        email: '',
        password: ''
      },
      validate: false
    };
  },
  methods: {
    setProfile() {
      this.validate = true;
      this.$emit('setProfile', this.hasError ? {} : this.profileItems);
    }
  },
  computed: {
    profileItems() {
      return [
        {
          name: 'name',
          input: this.profileItemsData.name
        },
        {
          name: 'email',
          input: this.profileItemsData.email
        },
        {
          name: 'password',
          input: this.profileItemsData.password
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
            [this.profileItemsData.name, '入力されていません。']
          ),
        },
        {
          name: 'email',
          type: 'email',
          errors: validationErrors(
            [this.profileItemsData.email, '入力されていません。']
          ),
        },
        {
          name: 'password',
          type: 'password',
          errors: validationErrors(
            [this.profileItemsData.password, '入力されていません。'],
            [this.profileItemsData.password.length >= 8, 'パスワードは8文字以上です。']
          ),
        }    
      ];
    }
  }
};

</script>
