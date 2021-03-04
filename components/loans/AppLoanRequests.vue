<template lang="pug">
v-card(flat)
  v-simple-table(v-if="loans.length")
    template(v-slot:default)
      thead
        tr
          th Libro
          th Autor
          th Solicitado por
          th Fecha de solicitud
          th Opciones
      tbody
        tr(v-for="loan in loans" :key="loan.id")
          td {{ loan.books[0].name }}
          td {{ loan.books[0].author }}
          td {{ loan.user.email }}
          td
            TimeAgo(:datetime="loan.createdAt")
          td
            AppLoanRequestsApprove(:id="loan.id" @approved="removeLoan")
            AppLoanRequestsReject(:id="loan.id" @rejected="removeLoan")
  v-card-text(v-else) No hay solicitudes pendientes
</template>

<script>
import TimeAgo from '@/components/TimeAgo'
import AppLoanRequestsApprove from '@/components/loans/AppLoanRequestsApprove'
import AppLoanRequestsReject from '@/components/loans/AppLoanRequestsReject'

export default {
  components: { TimeAgo, AppLoanRequestsApprove, AppLoanRequestsReject },
  data: () => ({
    loans: [],
    loading: false,
  }),
  async fetch() {
    try {
      this.loading = true
      this.loans = await this.$axios.$get('loans/type?pending=true')
    } catch (error) {
      console.log(error)
    } finally {
      this.loading = false
    }
  },
  methods: {
    removeLoan(id) {
      this.loans = this.loans.filter((item) => item.id !== id)
    },
  },
}
</script>
