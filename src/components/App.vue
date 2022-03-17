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
      profileItems: {
        name: {
          input: '',
          errorMessages: ['']
        },
        email: {
          input: '',
          errorMessages: ['']
        },
        password: {
          input: '',
          errorMessages: ['']
        }
      }
    };
  },
  methods: {
    setProfile: function(profileItems) {
      this.profileItems.name = profileItems.name;
      this.profileItems.email = profileItems.email;
      this.profileItems.password = profileItems.password;
    }
  },
};
</script>