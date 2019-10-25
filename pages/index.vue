<template lang="pug">
section.section
  .container
    .columns
      .column
        h1.title.is-4
          | らんちゅうウォッチ
    .columns.is-mobile
      .column
        time
          | {{date}}
      .column.has-text-right
        button.button(@click="reload()")
          | 更新する
    .columns
      .column
        figure.image
          img(:src="ranchuNow")
    .columns
      .column
        p.has-text-centered
          | &copy; funnythingz
</template>

<script>
export default {

  computed: {
    date() {
      return `${this.year()}/${this.month()}/${this.day()} ${this.hour()}:${this.minutes()}`
    },
    ranchuNow() {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.year()}${this.month()}${this.day()}`
      const hourDir = `${this.hour()}`
      const filename = `${this.year()}${this.month()}${this.day()}-${this.hour()}${this.minutes()}.jpg`
      return `${domain}/${dateDir}/${hourDir}/${filename}` 
    }
  },

  methods: {

    now() {
      return new Date(new Date() - (1000 * 60))
    },

    year() {
      return this.now().getFullYear()
    },

    month() {
      const month = this.now().getMonth() + 1
      if (month < 10) {
        return `0${month}`
      }
      return month
    },

    day() {
      return this.now().getDate()
    },

    hour() {
      const hour = this.now().getHours()
      if (hour < 10) {
        return `0${hour}`
      }
      return hour
    },

    minutes() {
      const minutes = this.now().getMinutes()
      if (minutes < 10) {
        return `0${minutes}`
      }
      return minutes
    },

    reload() {
      location.reload()
    }

  }
}
</script>

<style lang="sass" scoped>
.container
  margin-top: 1rem
.image > img
</style>
