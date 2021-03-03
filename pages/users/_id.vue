<template lang="pug">
v-card(:loading="$fetchState.pending || loading" outlined)
  v-card-text
    v-alert(v-model="error" type="error" text dismissible) ¡Hubo un error!
    v-alert(v-model="success" type="success" text dismissible) ¡Usuario actualizado correctamente!
    v-row
      v-col(:cols="8")
        v-text-field(:value="user.email" label="Correo" outlined disabled)
        v-text-field(:value="user.name" label="Nombre" outlined disabled)
        v-select(:items="roles" :value="user.role" @change="saveUser" label="Rol" outlined)
      v-col(:cols="4")
        v-card.mb-4(outlined)
          v-card-title Fecha de creación
          v-card-text
            TimeAgo(:datetime="user.createdAt")
        v-card(outlined)
          v-card-title Fecha de actualización
          v-card-text
            TimeAgo(:datetime="user.updatedAt")
</template>

<script>
import TimeAgo from '@/components/TimeAgo'
export default {
  components: { TimeAgo },
  data: () => ({
    user: 'hola',
    roles: ['admin', 'bibliotecario', 'lector'],
    success: false,
    error: false,
    loading: false,
  }),
  async fetch() {
    try {
      const user = await this.$axios.$get(`users/${this.$route.params.id}`)
      console.log({ user })

      this.user = user
    } catch (error) {
      console.error(error)
      this.$router.back()
    }
  },
  methods: {
    async saveUser(role) {
      try {
        this.success = false
        this.error = false
        this.loading = true

        await this.$axios.$put(`users/${this.$route.params.id}`, { role })

        this.success = true
      } catch (error) {
        this.error = true
        console.log(error)
      } finally {
        this.loading = false
      }
    },
  },
}
</script>
