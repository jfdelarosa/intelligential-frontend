<template lang="pug">
v-card(:loading="loading" outlined)
    v-card-title Nuevo libro
    v-card-text
      v-text-field(v-model="book.name" label="Nombre" placeholder="Nombre" outlined)
      v-textarea(v-model="book.description" label="Descripción" placeholder="Descripción" outlined)
    v-card-actions
      v-spacer
      v-btn(color="success" text @click="save") Guardar
</template>

<script>
export default {
  data: () => ({
    book: {
      name: '',
      description: '',
    },
    loading: false,
  }),
  methods: {
    async save() {
      try {
        this.loading = true
        const requiredFields = ['name']

        for (let key in this.book) {
          if (this.book[key] === '' && requiredFields.includes(key)) {
            throw new Error('Missing field')
          }
        }

        await this.$axios.$post('books', this.book)
        this.$router.push({ name: 'books' })
      } catch (error) {
        console.error()
      } finally {
        this.loading = true
      }
    },
  },
}
</script>
