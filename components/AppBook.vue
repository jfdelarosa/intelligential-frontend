<template lang="pug">
v-card(:to="{ name: 'books-id', params: { id: book.id } }" :loading="loading" :disabled="loading || requested" outlined)
  v-card-title {{ book.name }}
  v-card-text {{ book.description }}
  v-card-actions
    v-spacer
    v-btn(color="success" @click.prevent="loan(book.id)" text) Solicitar préstamo
</template>

<script>
export default {
  props: {
    book: {
      type: Object,
      default: () => ({}),
    },
  },
  data: () => ({
    loading: false,
    requested: false,
  }),
  methods: {
    async loan(id) {
      try {
        this.loading = true
        await this.$axios.$post('loans', {
          books: [id],
        })

        this.$router.push({ name: 'myloans' })
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
        this.requested = true
      }
    },
  },
}
</script>
