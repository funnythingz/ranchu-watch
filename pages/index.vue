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
            | {{todayDate}}
  .container
    h1.title.is-6
      | いまのらんちゅう
    .columns
      .column
        figure.image
          img(:src="ranchuNow" v-if="isOnTime()")
  .container(v-if="isOnToday()")
    h1.title.is-6
      | きょうのらんちゅう
    .columns.is-variable.is-1.is-mobile.is-multiline
      .column.is-3.has-text-centerd(v-for="archive in archives" v-if="parseInt(archive.h + archive.m) <= parseInt(todayHour() + todayMinutes())")
        figure.thum
          img(:src="todayFileName(archive.h, archive.m)")
        p.is-size-7.has-text-centered
          | {{archive.h}}:{{archive.m}}
  .container
    h1.title.is-6
      | きのうのらんちゅう
    .columns.is-variable.is-1.is-mobile.is-multiline
      .column.is-3.has-text-centerd(v-for="archive in archives")
        figure.thum
          img(:src="yesterdayFileName(archive.h, archive.m)")
        p.is-size-7.has-text-centered
          | {{archive.h}}:{{archive.m}}
</template>

<script>
export default {

  data() {
    return {
      archives: [],
      today: new Date(new Date() - (1000 * 60)),
      yesterday: new Date(new Date() - (((1000 * 60) * 60) * 24))
    }
  },

  computed: {

    todayDate() {
      return `${this.todayYear()}/${this.todayMonth()}/${this.todayDay()} ${this.todayHour()}:${this.todayMinutes()}`
    },

    ranchuNow() {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.todayYear()}${this.todayMonth()}${this.todayDay()}`
      const filename = `${dateDir}-${this.todayHour()}${this.todayMinutes()}.jpg`
      return `${domain}/${dateDir}/${this.todayHour()}/${filename}`
    }

  },

  created() {
    this.createArchives()
  },

  methods: {

    todayYear() {
      return this.today.getFullYear()
    },

    todayMonth() {
      const month = this.today.getMonth() + 1
      if (month < 10) {
        return `0${month}`
      }
      return month.toString()
    },

    todayDay() {
      const day = this.today.getDate()
      if (day < 10) {
        return `0${day}`
      }
      return day.toString()
    },

    todayHour() {
      const hour = this.today.getHours()
      if (hour < 10) {
        return `0${hour}`
      }
      return hour.toString()
    },

    todayMinutes() {
      const minutes = this.today.getMinutes()
      if (minutes < 10) {
        return `0${minutes}`
      }
      return minutes.toString()
    },

    yesterdayYear() {
      return this.yesterday.getFullYear()
    },

    yesterdayMonth() {
      const month = this.yesterday.getMonth() + 1
      if (month < 10) {
        return `0${month}`
      }
      return month.toString()
    },

    yesterdayDay() {
      const day = this.yesterday.getDate()
      if (day < 10) {
        return `0${day}`
      }
      return day.toString()
    },

    yesterdayHour() {
      const hour = this.yesterday.getHours()
      if (hour < 10) {
        return `0${hour}`
      }
      return hour.toString()
    },

    yesterdayMinutes() {
      const minutes = this.yesterday.getMinutes()
      if (minutes < 10) {
        return `0${minutes}`
      }
      return minutes.toString()
    },

    reload() {
      location.reload()
    },

    isOnTime() {
      return (new Date().getHours() >= 8 && new Date().getHours() < 16)
    },

    isOnToday() {
      return (800 <= parseInt(this.todayHour() + this.todayMinutes()))
    },

    todayFileName(h, m) {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.todayYear()}${this.todayMonth()}${this.todayDay()}`
      const filename = `${dateDir}-${h}${m}.jpg`

      return `${domain}/${dateDir}/${h}/${filename}`
    },

    yesterdayFileName(h, m) {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.yesterdayYear()}${this.yesterdayMonth()}${this.yesterdayDay()}`
      const filename = `${dateDir}-${h}${m}.jpg`

      return `${domain}/${dateDir}/${h}/${filename}`
    },

    createArchives() {
      for(var i = 8; i < 16; i++) {
        var _i = i
        if (_i < 10) {
          _i = `0${i}`
        }
        var __i = _i.toString()
        this.archives.push({h: __i, m: '00'})
        this.archives.push({h: __i, m: '15'})
        this.archives.push({h: __i, m: '30'})
        this.archives.push({h: __i, m: '59'})
      }
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
