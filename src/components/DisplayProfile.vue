<template>
  <template v-if="DoesProfilePassCheck">
    <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
      Your {{ profileItemLabel }} is 
      {{ profileItemLabel === 'password'
          ? profileItem.input.replace(/\S/g, '●')
          : profileItem.input }}
    </p>
  </template>
</template>

<script>
export default {
  props: ['profileItems'],
  computed: {
    DoesProfilePassCheck: function() {
      return Object.values(this.profileItems)
        .every(profileItem => !profileItem.errorMessages.length);
    }
  }
};
</script>