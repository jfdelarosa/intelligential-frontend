<template lang="pug">
v-dialog(v-model="dialog" width="500")
  template(v-slot:activator="{ on, attrs }")
    v-btn(v-on="on" v-bind="attrs" color="success" icon)
      v-icon mdi-check
  v-card(:loading="loading" :disabled="loading")
    v-card-title Aprobar solicitud de préstamo
    v-card-text
      AppDatePicker(v-model="startDate" label="Fecha de inicio del préstamo")
      AppDatePicker(v-model="endDate" label="Fecha de devolución" :minDate="minDate")
    v-card-actions
      v-spacer
      v-btn(@click="dialog = false" text) Cancelar
      v-btn(@click="approve" color="success" depressed)
        v-icon(left) mdi-check
        | Aprobar solicitud
</template>

<script>
import AppDatePicker from '@/components/AppDatePicker'
import { parseISO } from 'date-fns'

export default {
  components: { AppDatePicker },
  props: {
    id: {
      type: String,
    },
  },
  data: () => ({
    dialog: false,
    loading: false,
    startDate: '',
    endDate: '',
  }),
  computed: {
    minDate() {
      let date = parseISO(this.startDate)
      date.setDate(date.getDate() + 2)

      return date
    },
  },
  methods: {
    formatDate(date) {
      return new Date(date).toISOString().substring(0, 10)
    },
    async approve() {
      try {
        this.loading = true
        const startDate = this.startDate
        const endDate = this.endDate

        if (startDate === '' || endDate === '') {
          return
        }

        await this.$axios.$put(`loans/${this.id}`, {
          startDate,
          endDate,
          approved: true,
          returned: false,
        })
        this.$emit('approved', this.id)
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
        this.dialog = false
      }
    },
  },
  mounted() {
    this.startDate = this.formatDate(new Date())
  },
}
</script>
