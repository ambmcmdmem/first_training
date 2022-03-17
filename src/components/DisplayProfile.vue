<template>
  <template v-if="isProfilefilled">
    <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
      Your {{ profileItemLabel }} is {{ profileItem.input }}
    </p>
  </template>
</template>

<script>
export default {
  props: ['profileItems'],
  computed: {
    isProfilefilled: function() {
      return Object.values(this.profileItems)
        .every(profileItem => profileItem.errorMessages
          .every(errorMessages => !errorMessages));
    }
  }
};
</script>