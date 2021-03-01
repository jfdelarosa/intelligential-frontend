<template lang="pug">
v-row(justify="center" align="center")
  v-col(cols="12" sm="8" md="6")
    v-card
      v-card-title Login
      v-card-text
        v-text-field(v-model="login.email" placeholder="Correo" outlined)
        v-text-field(v-model="login.password" placeholder="Contraseña" outlined)
        v-btn(@click="doLogin") Login
</template>

<script>
export default {
  layout: 'full',
  data: () => ({
    login: {
      email: '',
      password: '',
    },
  }),
  methods: {
    async doLogin() {
      try {
        let response = await this.$auth.loginWith('local', { data: this.login })
      } catch (error) {
        console.error(error)
      }
    },
  },
}
</script>
