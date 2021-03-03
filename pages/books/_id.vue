<template lang="pug">
v-card(:loading="$fetchState.pending" outlined)
  v-card-title {{ book.name }}
  v-card-text
    pre {{ book }}
</template>

<script>
export default {
  data: () => ({
    book: {},
  }),
  async fetch() {
    this.book = await this.$axios.$get(`books/${this.$route.params.id}`)
  },
}
</script>
