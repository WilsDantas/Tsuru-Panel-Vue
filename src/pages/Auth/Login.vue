<template>
  <div class="container-form">
    <div class="left">
      <div class="header">
        <h2>Welcome Back</h2>
        <h4>Log in to your account using email and password</h4>
      </div>

      <form @submit.prevent="login">
        <div class="text-danger" v-if="errors.invalid != undefined">{{this.errors.invalid[0]}}</div>
        <div class="text-danger" v-if="errors.email != undefined">{{this.errors.email[0]}}</div>
        <input
          type="email"
          :class="{'error': errors.email}"
          class="form-field"
          placeholder="Email Address"
          v-model="formData.email"
        />
        <div class="text-danger" v-if="errors.password != undefined">{{this.errors.password[0]}}</div>
        <input
          type="password"
          :class="{'error': errors.password}"
          class="form-field"
          placeholder="Password"
          v-model="formData.password"
        />
        <p>
          <a href="#">Forgot Password</a>
        </p>
        <button v-if="loading">LOGIN</button>
        <button v-else disabled><i class="fas fa-spinner fa-pulse"></i></button>
      </form>
    </div>
    <div class="right"></div>
  </div>
</template>

<script>
import { mapState } from "vuex";
export default {
  created() {
    this.$store.dispatch('checkLogin')
      .then(() => this.$router.push({ name: "dashboard" }))
      .catch(() => '')
  },
  data() {
    return {
      loading: true,
      formData: {
        device_name:
          navigator.appCodeName + navigator.appName + navigator.platform,
      },
      errors: {},
    };
  },
  methods: {
    login() {
      this.loading = false;
      this.$store
        .dispatch("login", this.formData)
        .then((response) => {
          this.$router.push({ name: "dashboard" })
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        })
        .finally(() => (this.loading = true));
    },
  },
};
</script>