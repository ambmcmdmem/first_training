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
        input: this.name
      }
    },
    emailInfo: function() {
      return {
        input: this.email
      }
    },
    passwordInfo: function() {
      return {
        input: this.password.replace(/\S/g, '●')
      }
    },
    profileItems: function() {
      return {
        name: this.nameInfo,
        email: this.emailInfo,
        password: this.passwordInfo
      }
    }
  },
};
</script>