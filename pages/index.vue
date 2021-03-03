<template lang="pug">
v-card(:loading="loading" outlined)
  v-card-title Búsqueda
  v-card-text
    v-text-field(v-model="query" @input="search" placeholder="Buscar libro" outlined)
    v-row
      v-col(:cols="4" v-for="book in results" :key="book.id")
        v-card(@click="view(book.id)" outlined)
          v-card-title {{ book.name }}
          v-card-text {{ book.description }}
          v-card-actions
            v-spacer
            v-btn(color="success" @click="loan(book.id)" text) Solicitar préstamo
</template>

<script>
export default {
  data: () => ({
    query: '',
    timer: null,
    results: [],
    loading: false,
  }),
  watch: {
    query(val) {
      if (!val) {
        this.results = []
        return
      }

      this.debouncedSearch()
    },
  },
  methods: {
    debouncedSearch() {
      clearTimeout(this.timer)

      this.timer = setTimeout(async () => {
        await this.search()
      }, 500)
    },
    async search() {
      try {
        this.loading = true
        this.results = await this.$axios.$get(`books/search?q=${this.query}`)
      } catch (error) {
        console.error(error)
      } finally {
        this.loading = false
      }
    },
    async loan(id) {
      // await this.
    },
    view(id) {
      this.$router.push({
        name: 'books-id',
        params: { id },
      })
    },
  },
}
</script>
