<template lang="pug">
v-dialog(v-model="dialog" width="500")
  template(v-slot:activator="{ on, attrs }")
    v-btn(v-on="on" v-bind="attrs" color="error" icon)
      v-icon mdi-close
  v-card(:loading="loading" :disabled="loading")
    v-card-title Rechazar solicitud de préstamo
    v-card-text ¿Deseas rechazar la solicitud?
    v-card-actions
      v-spacer
      v-btn(@click="dialog = false" text) Cancelar
      v-btn(@click="remove" color="error" depressed)
        v-icon(left) mdi-close
        | Rechazar solicitud
</template>

<script>
export default {
  props: {
    id: {
      type: String,
    },
  },
  data: () => ({
    dialog: false,
    loading: false,
  }),
  methods: {
    async remove() {
      try {
        this.loading = true
        await this.$axios.$put(`loans/${this.id}`, {
          approved: false,
          returned: true,
        })
        this.$emit('rejected', this.id)
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
        this.dialog = false
      }
    },
  },
}
</script>
