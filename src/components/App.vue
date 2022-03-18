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
    setProfile: function(profileItems) {
      this.name = profileItems.name;
      this.email = profileItems.email;
      this.password = profileItems.password;
    }
  },
  computed: {
    profileItems: function() {
      return {
        name: this.name,
        email: this.email,
        password: this.password
      }
    }
  },
};
</script>