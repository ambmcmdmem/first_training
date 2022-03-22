<template>
  <template v-if="Object.keys(profile).length">
    <p>Your Name is {{ profile.name }}</p>
    <p>Your Email is {{ profile.email }}</p>
    <p>Your Password is {{ passwordToBlackCircle }}</p>
  </template>
</template>

<script>
import {emitter} from '../emitter';

export default {
  data() {
    return {
      profile: {}
    };
  },
  computed: {
    passwordToBlackCircle() {
      return this.profile.password.replace(/\S/g, '●');
    }
  },
  mounted() {
    emitter.on('setProfile', profile => {
      this.profile = profile;
    });
  }
};
</script>