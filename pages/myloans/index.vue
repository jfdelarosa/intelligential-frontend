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
        | Solicitudes pendientes
        v-icon mdi-view-list

      v-tab
        | En curso
        v-icon mdi-calendar-month

      v-tab
        | Historial
        v-icon mdi-history

    v-tabs-items
      AppMyPendingLoans(v-if="tab === 0")
      AppCurrentLoans(v-if="tab === 1")
      AppMyHistory(v-if="tab === 2")

</template>

<script>
import AppMyPendingLoans from '@/components/myloans/AppMyPendingLoans'
import AppCurrentLoans from '@/components/myloans/AppCurrentLoans'
import AppMyHistory from '@/components/myloans/AppMyHistory'

export default {
  components: { AppMyPendingLoans, AppCurrentLoans, AppMyHistory },
  data: () => ({
    books: [],
    tab: 0,
  }),
  async fetch() {},
  methods: {},
}
</script>
