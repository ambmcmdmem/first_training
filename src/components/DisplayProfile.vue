<template>
  <template v-if="DoesProfilePassCheck" v-cloak>
    <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
      Your {{ profileItemLabel }} is 
      {{ profileItem }}
    </p>
  </template>
</template>

<script>
export default {
  props: ['profileItems'],
  computed: {
    DoesProfilePassCheck: function() {
      return Object.values(this.profileItems)
        .every(profileItem => profileItem);
    }
  }
};
</script>