<template lang="pug">
  v-tooltip(bottom)
    template(v-slot:activator="{ on, attrs }")
      span(v-bind="attrs" v-on="on") {{currentDate}}
    span {{currentDateRelative}}
</template>

<script>
import { formatDistanceToNowStrict, formatRelative } from 'date-fns'

export default {
  props: {
    datetime: {
      required: false,
      type: [String, Number],
      default: Date.now(),
    },
  },
  data: () => ({
    interval: null,
    currentDate: null,
    currentDateRelative: null,
  }),
  methods: {
    parse(datetime) {
      this.currentDate = formatDistanceToNowStrict(datetime, {
        addSuffix: true,
      })
      this.currentDateRelative = formatRelative(datetime, new Date())
    },
  },
  mounted() {
    const datetime = new Date(this.datetime)

    this.parse(datetime)

    this.interval = setInterval(() => {
      this.parse(datetime)
    }, 1000)
  },
  destroyed() {
    clearInterval(this.interval)
  },
}
</script>
