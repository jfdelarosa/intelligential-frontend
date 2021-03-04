<template lang="pug">
v-card(:loading="loading" outlined)
  v-card-title Búsqueda
  v-card-text
    v-text-field(
      v-model="query" placeholder="Buscar libro" outlined)
    v-row(v-if="results.length")
      v-col(:cols="4" v-for="book in results" :key="book.id")
        AppBook(:book="book")
    v-card-text(v-if="query && results.length === 0 && !loading") Sin resultados
</template>

<script>
import AppBook from '@/components/AppBook'

export default {
  components: { AppBook },
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
      this.loading = true
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
  },
}
</script>
