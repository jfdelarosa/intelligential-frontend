<template lang="pug">
v-app
  v-navigation-drawer(v-model="drawer" clipped fixed app)
    v-list
      template(v-for="(item, i) in items")
        v-subheader(v-if="item.header") {{ item.header }}
        v-list-item(router exact :to="{ name: item.name }" v-else)
          v-list-item-action
            v-icon {{ item.icon }}
          v-list-item-content
            v-list-item-title {{ item.title }}
      v-divider
      v-list-item(@click="$auth.logout()")
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
          header: 'Administración',
        },
        {
          icon: 'mdi-account-group',
          title: 'Usuarios',
          name: 'users',
        },
        {
          icon: 'mdi-clipboard-list-outline',
          title: 'Inventario',
          name: 'books',
        },
        {
          header: 'Lector',
        },
        {
          icon: 'mdi-magnify',
          title: 'Búsqueda de libros',
          name: 'index',
        },
      ],
    }
  },
}
</script>

<style>
.v-main__wrap {
  background: #f3f4f7;
}
</style>
