<template>
  <div class="login-page">
    <div class="login-card">
      <i class="fas fa-graduation-cap fa-3x" style="color: var(--primary); margin-bottom: 20px;"></i>
      <h2>Welcome to EduManage</h2>
      <p style="color: var(--gray); margin-bottom: 30px;">Enter your credentials to access the dashboard.</p>
      <div class="form-group">
        <input type="email" class="form-control" placeholder="Email" v-model="email">
      </div>
      <div class="form-group">
        <input type="password" class="form-control" placeholder="Password" v-model="password">
      </div>
      <button class="btn btn-primary" @click="handleLogin" style="width: 100%;">Login</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  data() {
    return {
      email: 'admin@edumanage.com',
      password: 'password'
    };
  },
  methods: {
    handleLogin() {
      if (this.email && this.password) {
        localStorage.setItem('isLoggedIn', 'true'); // Simulate session
        this.$emit('login-success');
      } else {
        alert('Please enter both email and password.');
      }
    }
  }
};
</script>

<style scoped>
/* ... نفس الاستايلات ... */
</style>