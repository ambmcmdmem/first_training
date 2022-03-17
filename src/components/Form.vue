<template>
  <label style="display: block;" v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-input'">
    {{ profileItemLabel }}:
    <input :type="profileItemLabel" :ref="profileItemLabel">
    <template v-if="validate">
      <template v-for="(errorMessage, errorMessageNo) in profileItem.errorMessages" :key="profileItemLabel + '-error-' + errorMessageNo">
        <span v-if="errorMessage" style="color: red" v-cloak>
          {{ errorMessage }}
        </span>
      </template>
    </template>
  </label>
  <button @click="setProfile" type="button">SUBMIT</button>
</template>

<script>
export default {
  props: ['profileItems', 'validate'],
  data() {
    return {
      validate: false
    }
  },
  methods: {
    setProfile() {
      this.$emit(
        'setProfile',
        this.$refs.name[0].value,
        this.$refs.email[0].value,
        this.$refs.password[0].value);
      this.validate = true;
    },
  }
};
</script>