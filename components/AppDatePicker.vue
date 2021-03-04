<template lang="pug">
v-menu(
  ref="menu"
  v-model="isOpen"
  :close-on-content-click="false"
  transition="scale-transition"
  offset-y
  min-width="auto"
)
  template(v-slot:activator="{ on, attrs }")
    v-text-field(
      :value="value"
      :label="label"
      prepend-inner-icon="mdi-calendar"
      readonly
      outlined
      v-bind="attrs"
      v-on="on"
    )
  v-date-picker(
    :value="value"
    :min="minDateFormated"
    @change="onChange"
    scrollable
  )
</template>

<script>
export default {
  props: {
    label: {
      type: String,
    },
    value: {
      type: String,
    },
    minDate: {
      type: Date,
      default: () => new Date(),
    },
  },
  data: () => ({
    isOpen: false,
  }),
  computed: {
    minDateFormated() {
      return new Date(this.minDate).toISOString().substring(0, 10)
    },
  },
  methods: {
    onChange(val) {
      this.isOpen = false
      this.$emit('input', val)
    },
  },
}
</script>
