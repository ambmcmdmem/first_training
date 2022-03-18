<template>
  <Form
    :profileItems='profileItems'
    @setProfile="setProfile"
  ></Form>

  <DisplayProfile
    :profileItems="profileItems"
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
    setProfile(profileItems) {
      this.name = profileItems.name;
      this.email = profileItems.email;
      this.password = profileItems.password;
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
      }
    }
  },
};
</script>