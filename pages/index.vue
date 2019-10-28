<template lang="pug">
section.section
  .container.wrapper
    .columns
      .column.is-8
        .container
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
          section.spacing
            h1.title.is-6
              | いまのらんちゅう
            .columns
              .column
                figure.image
                  img(:src="ranchuNow" v-if="isOnTime()")
          section.spacing(v-if="isOnToday()")
            h1.title.is-6
              | きょうのらんちゅう
            .columns.is-variable.is-1.is-mobile.is-multiline
              .column.is-3.has-text-centerd(v-for="archive in archives" v-if="parseInt(archive.h + archive.m) <= parseInt(hour() + minutes())")
                figure.thum
                  img(:src="fileName(archive.h, archive.m, 0)")
                p.is-size-7.has-text-centered
                  | {{archive.h}}:{{archive.m}}
          section.spacing
            h1.title.is-6
              | きのうのらんちゅう
            .columns.is-variable.is-1.is-mobile.is-multiline
              .column.is-3.has-text-centerd(v-for="archive in archives")
                figure.thum
                  img(:src="fileName(archive.h, archive.m, -1)")
                p.is-size-7.has-text-centered
                  | {{archive.h}}:{{archive.m}}
      .column.is-4
        section
          .container
            .columns
              .column
                article.message
                  .message-body
                    | こんにちわんわん
</template>

<script>
export default {

  data() {
    return {
      archives: []
    }
  },

  computed: {

    date() {
      return `${this.year()}/${this.month()}/${this.day()} ${this.hour()}:${this.minutes()}`
    },

    ranchuNow() {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.year()}${this.month()}${this.day()}`
      const filename = `${dateDir}-${this.hour()}${this.minutes()}.jpg`
      return `${domain}/${dateDir}/${this.hour()}/${filename}`
    }

  },

  created() {
    this.createArchives()
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
      return month.toString()
    },

    day() {
      const day = this.now().getDate()
      if (day < 10) {
        return `0${day}`
      }
      return day.toString()
    },

    hour() {
      const hour = this.now().getHours()
      if (hour < 10) {
        return `0${hour}`
      }
      return hour.toString()
    },

    minutes() {
      const minutes = this.now().getMinutes()
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
      return (800 <= parseInt(this.hour() + this.minutes()))
    },

    fileName(h, m, diffDay) {
      const domain = 'https://ranchu-watch.s3-ap-northeast-1.amazonaws.com/capture'
      const dateDir = `${this.year()}${this.month()}${parseInt(this.day()) + diffDay}`
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
    vertical-align: middle
    z-index: 1

.thum
  width: 100%
  > img
    vertical-align: middle

.title
  margin-bottom: 10px

.spacing
  margin-top: 1rem

.wrapper
</style>
