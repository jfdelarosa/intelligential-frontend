<template lang="pug">
v-card(outlined)
    v-tabs(
      v-model="tab"
      background-color="primary"
      dark
      fixed-tabs
      icons-and-text
    )
      v-tabs-slider

      v-tab
        | Solicitudes
        v-icon mdi-view-list

      v-tab
        | En curso
        v-icon mdi-calendar-month

      v-tab
        | Historial
        v-icon mdi-history

    v-tabs-items
      AppLoanRequests(v-if="tab === 0")
      AppLoanCurrent(v-if="tab === 1")
      AppLoanHistory(v-if="tab === 2")

</template>

<script>
import AppLoanRequests from '@/components/loans/AppLoanRequests'
import AppLoanCurrent from '@/components/loans/AppLoanCurrent'
import AppLoanHistory from '@/components/loans/AppLoanHistory'

export default {
  components: { AppLoanRequests, AppLoanCurrent, AppLoanHistory },
  data: () => ({
    books: [],
    tab: 'tab-1',
  }),
  async fetch() {},
  methods: {},
}
</script>
