<template lang="pug">
section.section
  .container
    .columns
      .column
        .columns.is-mobile.is-vcentered
          .column
            h1.title.is-4
              | らんちゅうウォッチ
          .column.has-text-right.is-3
            button.button.is-small(@click="reload()")
              | 更新
    .columns.is-mobile.is-vcentered
      .column
        p
          | ライブ時間: 8:00 ~ 16:00
      .column.is-4.has-text-right
        p.is-size-7
          time
            | {{date}}
  .container
    h1.title.is-6
      | いまのらんちゅう
    .columns
      .column
        figure.image
          img(:src="ranchuNow" v-if="isOnTime()")
  .container(v-if="isOnTime()")
    h1.title.is-6
      | きょうのらんちゅう
    .columns.is-variable.is-1.is-mobile.is-multiline
      .column.is-3.has-text-centerd(v-for="archive in archives" v-if="parseInt(archive.h) <= parseInt(hour())")
        figure.thum
          img(:src="fileName(archive.h, archive.m, 0)")
        p.is-size-7.has-text-centered
          | {{archive.h}}:{{archive.m}}
  .container
    h1.title.is-6
      | きのうのらんちゅう
    .columns.is-variable.is-1.is-mobile.is-multiline
      .column.is-3.has-text-centerd(v-for="archive in archives")
        figure.thum
          img(:src="fileName(archive.h, archive.m, -1)")
        p.is-size-7.has-text-centered
          | {{archive.h}}:{{archive.m}}
</template>

<script>
export default {

  data() {
    return {
      archives: [
        {h: '08', m: '00'},
        {h: '08', m: '30'},
        {h: '09', m: '00'},
        {h: '09', m: '30'},
        {h: '10', m: '00'},
        {h: '10', m: '30'},
        {h: '11', m: '00'},
        {h: '11', m: '30'},
        {h: '12', m: '00'},
        {h: '12', m: '30'},
        {h: '13', m: '00'},
        {h: '13', m: '30'},
        {h: '14', m: '00'},
        {h: '14', m: '30'},
        {h: '15', m: '00'},
        {h: '15', m: '30'}
      ]
    }
  },

  computed: {

    date() {
      return `${this.year()}/${this.month()}/${this.day()} ${this.hour()}:${this.minutes()}`
    },

    ranchuNow() {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.year()}${this.month()}${this.day()}`
      const hourDir = `${this.hour()}`
      const filename = `${dateDir}-${hourDir}${this.minutes()}.jpg`
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
    },

    isOnTime() {
      return (new Date().getHours() >= 8 && new Date().getHours() < 16)
    },

    fileName(h, m, diffDay) {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.year()}${this.month()}${this.day() + diffDay}`
      const filename = `${dateDir}-${h}${m}.jpg`

      return `${domain}/${dateDir}/${h}/${filename}`
    }

  }
}
</script>

<style lang="sass" scoped>
.container
  margin-top: 1rem
  max-width: 700px
.image
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

  > img
    position: relative
    z-index: 1

.thum
  width: 100%
  > img
    vertical-align: middle

.title
  margin-bottom: 10px
</style>
