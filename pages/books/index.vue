<template lang="pug">
div
  v-row
    v-spacer
    v-col(cols="auto")
      v-btn.mb-4(
        @click="newBook"
        color="success"
        depressed
      )
        v-icon mdi-plus
        | Nuevo
  v-card(:loading="$fetchState.pending" outlined)
    v-card-title Libros
    v-simple-table(v-if="books.length")
      template(v-slot:default)
        thead
          tr
            th SKU
            th Nombre
            th Autor
            th ¿En stock?
            th Fecha de registro
            th Último préstamo
            th Opciones
        tbody
          tr(v-for="book in books" :key="book.id")
            td {{ book.id }}
            td {{ book.name }}
            td {{ book.author }}
            td {{ book.stock ? "Sí" : "No" }}
            td
              TimeAgo(:datetime="book.createdAt")
            td
              TimeAgo(:datetime="book.updatedAt")
            td
              v-tooltip(bottom)
                template(v-slot:activator="{ on, attrs }")
                  v-btn(v-bind="attrs" v-on="on" @click="remove(book)" :color="book.stock ? 'red' : ''" icon)
                    v-icon mdi-delete-outline
                span(v-if="book.stock") Eliminar
                span(v-else) No puedes eliminar un libro hasta que sea devuelto

    v-card-text(v-else) No hay Libros registrados
</template>

<script>
import TimeAgo from '@/components/TimeAgo'

export default {
  components: { TimeAgo },
  data: () => ({
    books: [],
  }),
  async fetch() {
    this.books = await this.$axios.$get('books')
  },
  methods: {
    newBook() {
      this.$router.push({
        name: 'books-new',
      })
    },
    async remove({ id, stock }) {
      try {
        if (!stock) {
          return
        }
        await this.$axios.delete(`books/${id}`)
        this.books = this.books.filter((item) => item.id !== id)
      } catch (error) {}
    },
  },
}
</script>
