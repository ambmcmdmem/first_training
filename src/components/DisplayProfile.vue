<template>
  <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
    Your {{ profileItemLabel }} is {{ profileItem.input }}
  </p>
</template>

<script>
export default {
  props: ['profileItems']
};
</script>