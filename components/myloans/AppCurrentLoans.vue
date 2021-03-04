<template lang="pug">
v-card(flat)
  v-simple-table(v-if="loans.length")
    template(v-slot:default)
      thead
        tr
          th Libro
          th Autor
          th Fecha de solicitud
          th Inicio
          th Fin
      tbody
        tr(v-for="loan in loans" :key="loan.id")
          td {{ loan.books[0].name }}
          td {{ loan.books[0].author }}
          td
            TimeAgo(:datetime="loan.createdAt")
          td
            TimeAgo(:datetime="loan.startDate")
          td
            TimeAgo(:datetime="loan.endDate")
  v-card-text(v-else) No hay préstamos en curso
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
      this.loans = await this.$axios.$get('loans/my?pending=current')
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
