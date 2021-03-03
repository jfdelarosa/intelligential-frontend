<template lang="pug">
div
  v-row
    v-spacer
    v-col(cols="auto")
      v-btn.mb-4(
        @click="newUser"
        color="success"
        depressed
      )
        v-icon mdi-plus
        | Nuevo
  v-card(:loading="$fetchState.pending" outlined)
    v-card-title Usuarios
    v-simple-table(v-if="users.length")
      template(v-slot:default)
        thead
          tr
            th Correo
            th Rol
        tbody
          tr(v-for="user in users" :key="user.id" @click="viewUser(user.id)")
            td {{ user.email }}
            td {{ user.role }}
    v-card-text(v-else) No hay usuarios registrados
</template>

<script>
export default {
  data: () => ({
    users: [],
  }),
  async fetch() {
    try {
      this.users = await this.$axios.$get('users')
    } catch (error) {
      console.log(error)
    }
  },
  methods: {
    newUser() {
      this.$router.push({ name: 'users-new' })
    },
    viewUser(id) {
      this.$router.push({
        name: 'users-id',
        params: {
          id,
        },
      })
    },
  },
}
</script>
