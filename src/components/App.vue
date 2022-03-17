<template>
  <Form
    :profileItems='profileItems'
    @setProfile="setProfile"
  ></Form>

  <DisplayProfile
    :profileItems="profileItems"
    v-cloak
  ></DisplayProfile>
</template>

<script>
import Form from "./Form.vue";
import DisplayProfile from './DisplayProfile.vue';

export default {
  components: {
    Form,
    DisplayProfile
  },
  data() {
    return {
      name: '',
      email: '',
      password: '',
    };
  },
  methods: {
    setProfile: function(profileItems) {
      this.name = profileItems.name;
      this.email = profileItems.email;
      this.password = profileItems.password;
    }
  },
  computed: {
    nameInfo: function() {
      return {
        input: this.name,
        errorMessages: [
          this.name ? '' : '入力されていません。'
        ]
      }
    },
    emailInfo: function() {
      return {
        input: this.email,
        errorMessages: [
          this.email ? '' : '入力されていません。'
        ]
      }
    },
    passwordInfo: function() {
      return {
        input: this.password.replace(/\S/g, '●'),
        errorMessages: [
          this.password ? '' : '入力されていません。',
          this.password.length >= 8 ? '' : 'パスワードは8文字以上です。'
        ]
      }
    },
    profileItems: function() {
      return {
        name: this.nameInfo,
        email: this.emailInfo,
        password: this.passwordInfo
      }
    },
  },
};
</script>