<template>
  <Form
    :profile='profile'
    @setProfile="setProfile"
  ></Form>

  <DisplayProfile
    v-if="Object.keys(profile).length"
    :profile="profile"
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
      profile: {}
    };
  },
  methods: {
    setProfile(profile) {
      this.profile = profile;
    }
  }
};
</script>