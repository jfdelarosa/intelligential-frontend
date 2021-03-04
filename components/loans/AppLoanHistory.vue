<template lang="pug">
v-card(flat)
  v-simple-table(v-if="loans.length")
    template(v-slot:default)
      thead
        tr
          th Status
          th Libro
          th Autor
          th Solicitado por
          th Fecha de solicitud
          th Inicio
          th Fin
          th.text-center ¿Devuelto?
      tbody
        tr(v-for="loan in loans" :key="loan.id")
          td
            v-chip(v-if="loan.approved" color="success" small) Aprobada
            v-chip(color="error" small v-else) Rechazada
          td {{ loan.books[0].name }}
          td {{ loan.books[0].author }}
          td {{ loan.user.email }}
          td
            TimeAgo(:datetime="loan.createdAt")
          td
            TimeAgo(:datetime="loan.startDate" v-if="loan.startDate")
          td
            TimeAgo(:datetime="loan.endDate" v-if="loan.endDate")
          td.text-center {{ loan.returned ? "Sí" : "No" }}
  v-card-text(v-else) No hay elementos en el historial
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
      this.loans = await this.$axios.$get('loans/type?pending=false')
    } catch (error) {
      console.log(error)
    } finally {
      this.loading = false
    }
  },
}
</script>
