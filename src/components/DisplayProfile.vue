<template>
  <template v-if="DoesProfilePassCheck">
    <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
      Your {{ profileItemLabel }} is 
      {{ profileItem.input }}
    </p>
  </template>
</template>

<script>
export default {
  props: ['profileItems'],
  computed: {
    DoesProfilePassCheck: function() {
      console.log(Object.values(this.profileItems));
      return Object.values(this.profileItems)
        .every(profileItem => profileItem.input);
    }
  }
};
</script>