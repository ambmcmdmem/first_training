<template>
  <p v-for="profileItem in profileItems" :key="profileItem.name + '-information'">
    Your {{ profileItem.name }} is 
    {{ profileItem.name === 'password'
        ? profileItem.input.replace(/\S/g, '●')
        : profileItem.input }}
  </p>
</template>

<script>
export default {
  props: ['profileItems']
};
</script>