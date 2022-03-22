<template>
  <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
    Your {{ profileItemLabel }} is 
    {{ profileItemLabel === 'password'
      ? profileItem.replace(/\S/g, '●')
      : profileItem }}
  </p>
</template>

<script>
export default {
  props: ['profileItems']
};
</script>