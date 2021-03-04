<template lang="pug">
v-card(:loading="loading" outlined)
  v-card-title Nuevo usuario
  v-card-text
    v-alert(v-model="error" type="error" text dismissible) ¡Hubo un error!
    v-text-field(v-model="user.email" label="Correo" placeholder="john.doe@gmail.com" outlined)
    v-select(v-model="user.role" label="Rol" :items="roles" outlined)
    v-text-field(v-model="user.password" label="Contraseña" outlined)
    v-text-field(v-model="passwordRepeat" label="Repetir contraseña" outlined)
  v-card-actions
    v-spacer
    v-btn(color="success" text @click="save") Guardar
</template>

<script>
export default {
  data: () => ({
    user: {
      email: '',
      role: '',
      password: '',
    },
    passwordRepeat: '',
    roles: ['lector', 'bibliotecario', 'admin'],
    loading: false,
    error: false,
  }),
  methods: {
    async save() {
      try {
        this.loading = true
        this.error = false
        const requiredFields = ['email', 'role', 'password']

        for (let key in this.user) {
          if (this.user[key] === '' && requiredFields.includes(key)) {
            throw new Error('Missing field')
          }
        }

        if (this.user.password !== this.passwordRepeat) {
          throw new Error('Passwords do not match')
        }

        await this.$axios.$post('auth/register', this.user)
        this.$router.push({ name: 'users' })
      } catch (error) {
        this.error = true
        console.error(error)
      } finally {
        this.loading = false
      }
    },
  },
}
</script>
