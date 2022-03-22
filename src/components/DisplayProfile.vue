<template>
  <p>Your Name is {{ profile.name }}</p>
  <p>Your Email is {{ profile.email }}</p>
  <p>Your Password is {{ profile.password.replace(/\S/g, '●') }}</p>
</template>

<script>
export default {
  props: ['profile']
};
</script>