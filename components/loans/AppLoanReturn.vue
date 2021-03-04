<template lang="pug">
v-dialog(v-model="dialog" width="500")
  template(v-slot:activator="{ on, attrs }")
    v-btn(v-on="on" v-bind="attrs" color="success" icon)
      v-icon mdi-check
  v-card(:loading="loading" :disabled="loading")
    v-card-title Confirmar devolución
    v-card-text ¿Deseas confirmar de devuelto?
    v-card-actions
      v-spacer
      v-btn(@click="dialog = false" text) Cancelar
      v-btn(@click="confirm" color="success" depressed)
        v-icon(left) mdi-check
        | Confirmar
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
    async confirm() {
      try {
        this.loading = true

        await this.$axios.$put(`loans/${this.id}`, {
          returned: true,
        })
        this.$emit('returned', this.id)
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
