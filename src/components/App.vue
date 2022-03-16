<template>
  <div>
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

    <div v-if="isProfilefilled" v-cloak>
      <p v-for="(profileItem, profileItemLabel) in profileItems" :key="profileItemLabel + '-information'">
        Your {{ profileItemLabel }} is {{ profileItem.input }}
      </p>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
  data() {
    return {
      name: '',
      email: '',
      password: '',
      validate: false
    };
  },
  methods: {
    setProfile() {
      this.name = this.$refs.name[0].value;
      this.email = this.$refs.email[0].value;
      this.password = this.$refs.password[0].value;
      this.validate = true;
    },
  },
  computed: {
    nameInfo: function() {
      return {
        input: this.name,
        errorMessages: [
          this.name ? '' : '入力されていません。'
        ]
      }
    },
    emailInfo: function() {
      return {
        input: this.email,
        errorMessages: [
          this.email ? '' : '入力されていません。'
        ]
      }
    },
    passwordInfo: function() {
      return {
        input: this.password.replace(/\S/g, '●'),
        errorMessages: [
          this.password ? '' : '入力されていません。',
          this.password.length >= 8 ? '' : 'パスワードは8文字以上です。'
        ]
      }
    },
    profileItems: function() {
      return {
        name: this.nameInfo,
        email: this.emailInfo,
        password: this.passwordInfo
      }
    },
    isProfilefilled: function() {
      return Object.values(this.profileItems)
        .every(profileItem => profileItem.errorMessages
          .every(errorMessages => !errorMessages));
    }
  },
})
</script>