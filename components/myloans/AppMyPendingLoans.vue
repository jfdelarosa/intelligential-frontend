<template lang="pug">
v-card(:loading="loading" flat)
  v-simple-table(v-if="loans.length")
    template(v-slot:default)
      thead
        tr
          th Libro
          th Autor
          th Fecha de solicitud
      tbody
        tr(v-for="loan in loans" :key="loan.id")
          td {{ loan.books[0].name }}
          td {{ loan.books[0].author }}
          td
            TimeAgo(:datetime="loan.createdAt")
  v-card-text(v-else) No hay solicitudes pendientes
</template>

<script>
import TimeAgo from '@/components/TimeAgo'

export default {
  components: { TimeAgo },
  data: () => ({
    loans: [],
    loading: false,
  }),
  async fetch() {
    try {
      this.loading = true
      this.loans = await this.$axios.$get('loans/my?pending=true')
    } catch (error) {
      console.log(error)
    } finally {
      this.loading = false
    }
  },
}
</script>
