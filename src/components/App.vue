<template>
  <div>
    <Form :profileItems="profileItems" :validate="validate"></Form>
    <DisplayProfile v-if="isProfileFilled" :profileItems="profileItems" v-cloak></DisplayProfile>
  </div>
</template>

<script>
import { defineComponent } from '@vue/composition-api'
import Form from './Form.vue';
import DisplayProfile from './DisplayProfile.vue';

export default defineComponent({
  components: {
    Form,
    DisplayProfile
  },
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
    isProfileFilled: function() {
      return Object.values(this.profileItems)
        .every(profileItem => profileItem.errorMessages
          .every(errorMessages => !errorMessages));
    }
  },
})
</script>