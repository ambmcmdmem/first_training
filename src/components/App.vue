<template>
  <div>
    <ProfileForm></ProfileForm>
    <ProfileDisplay></ProfileDisplay>
  </div>
</template>

<script>
import ProfileForm from './ProfileForm.vue';
import ProfileDisplay from './ProfileDisplay.vue';

export default {
  components: {
    ProfileForm,
    ProfileDisplay,
  },
};
</script>
