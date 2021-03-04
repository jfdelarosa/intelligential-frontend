<template lang="pug">
v-row.login-container(justify="center" align="center")
  v-col(cols="12" sm="8" md="6" align-self="center")
    v-card(:loading="loading" outlined)
      v-card-title Login
      v-card-text
        v-alert(v-model="error" type="error" text dismissible) ¡Credenciales inválidas!
        v-text-field(v-model="login.email" placeholder="Correo" outlined)
        v-text-field(v-model="login.password" type="password" placeholder="Contraseña" outlined)
        v-btn(@click="doLogin" color="success" depressed block) Entrar
</template>

<script>
export default {
  layout: 'full',
  data: () => ({
    login: {
      email: '',
      password: '',
    },
    loading: false,
    error: false,
  }),
  methods: {
    async doLogin() {
      try {
        this.error = false
        this.loading = true

        if (this.login.email === '' || this.login.password === '') {
          throw new Error('Invalid credentials')
        }

        await this.$auth.loginWith('local', { data: this.login })
      } catch (error) {
        console.error(error)
        this.error = true
      } finally {
        this.loading = false
      }
    },
  },
}
</script>

<style>
.login-container {
  height: 100%;
}
</style>
