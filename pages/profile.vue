<template lang="pug">
v-card(:loading="loading" :disabled="loading" outlined)
  v-card-title Perfil
  v-card-text(v-if="error || success")
    v-alert(v-model="error" type="error" text dismissible) ¡Hubo un error!
    v-alert(v-model="success" type="success" text dismissible) ¡Usuario actualizado correctamente!
  v-card-text
    v-text-field(v-model="user.email" label="Correo electrónico" outlined)
    v-text-field(v-model="user.name" label="Nombre" outlined)
    v-text-field(v-model="user.role" label="Rol" disabled outlined)
  v-card-text Cambiar contraseña
  v-card-text
    v-text-field(v-model="user.password" type="password" label="Contraseña" outlined)
    v-text-field(v-model="password" type="password" label="Repetir contraseña" outlined)
  v-card-actions
    v-btn(@click="updateUser" color="success" depressed)
      v-icon(left) mdi-content-save
      | Actualizar
</template>

<script>
export default {
  data: () => ({
    user: {
      email: '',
      name: '',
      password: '',
    },
    password: '',
    loading: false,
    error: false,
    success: false,
  }),
  methods: {
    async updateUser() {
      try {
        this.error = false
        this.success = false
        this.loading = true

        if (this.user.password === '' && this.password === '') {
          delete this.user.password
        }

        if (this.user.password !== this.password) {
          throw new Error('Las contraseñas no coinciden')
        }

        await this.$axios.$put(`users/${this.user.id}`, this.user)
        await this.$auth.fetchUser()

        this.success = true
      } catch (error) {
        console.error(error)
        this.error = true
      } finally {
        this.loading = false
      }
    },
  },
  mounted() {
    this.user = { ...this.$auth.user }
    this.user.password = ''
  },
}
</script>
