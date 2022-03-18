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
      profileItems: {}
    };
  },
  methods: {
    setProfile(profileItems) {
      this.profileItems = profileItems;
    }
  }
};
</script>