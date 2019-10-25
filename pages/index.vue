<template lang="pug">
section.section
  .container
    .columns
      .column
        h1.title.is-4
          | らんちゅうウォッチ
        p
          | ライブ時間: 7:00 ~ 18:00
    .columns
      .column
        figure.image
          img(:src="ranchuNow")
        p.is-size-7.has-text-centered
          time
            | {{date}}
    .columns
      .column.has-text-centered
        button.button(@click="reload()")
          | 更新する
    .columns
      .column
        p.has-text-centered
            | &copy
            a.link(href="https://github.com/funnythingz" target="_blank")
              | funnythingz
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
  min-height: 240px
  background-color: #333
  position: relative
  &::before
    content: 'ライブ時間にまた来てね'
    color: #fff
    position: absolute;
    text-align: center;
    top: 45%;
    left: 20%;
    right: 20%;

.link
  margin-left: 5px
</style>
