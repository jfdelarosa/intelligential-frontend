<template lang="pug">
v-app
  v-navigation-drawer(v-model="drawer" clipped fixed app)
    v-list
      template(v-for="(item, i) in menu")
        v-subheader(v-if="item.header") {{ item.header }}
        v-list-item(router exact :to="{ name: item.name }" v-else)
          v-list-item-action
            v-icon {{ item.icon }}
          v-list-item-content
            v-list-item-title {{ item.title }}
      v-divider
      v-list-item(@click="logout")
        v-list-item-action
          v-icon mdi-logout
        v-list-item-content
          v-list-item-title Salir
  v-app-bar(color="primary" clipped-left fixed dark app)
    v-toolbar-title Reto Intelligential
  v-main
    v-container
      nuxt
</template>

<script>
export default {
  data() {
    return {
      drawer: true,
      fixed: false,
      items: [
        {
          icon: 'mdi-account-group',
          title: 'Usuarios',
          name: 'users',
          roles: ['admin'],
        },
        {
          icon: 'mdi-clipboard-list-outline',
          title: 'Inventario',
          name: 'books',
          roles: ['admin'],
        },
        {
          icon: 'mdi-information-outline',
          title: 'Gestión de préstamos',
          name: 'loans',
          roles: ['admin', 'bibliotecario'],
        },
        {
          icon: 'mdi-view-list',
          title: 'Mis préstamos',
          name: 'myloans',
          roles: ['admin', 'bibliotecario', 'lector'],
        },
        {
          icon: 'mdi-magnify',
          title: 'Búsqueda de libros',
          name: 'index',
          roles: ['admin', 'bibliotecario', 'lector'],
        },
      ],
    }
  },
  computed: {
    menu() {
      return this.items.filter((item) => {
        return item.roles.includes(this.$auth.user.role)
      })
    },
  },
  methods: {
    logout() {
      this.$auth.logout()
      this.$router.push('/login')
    },
  },
}
</script>

<style>
.v-main__wrap {
  background: #f3f4f7;
}
</style>
