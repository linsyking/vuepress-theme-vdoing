<template>
  <div
    class="body-bg"
    :style="`background: url(${bgImg}) center center / cover no-repeat;opacity:${opacity}`"
  ></div>
</template>

<script>
import { type } from '../util'
export default {
  data () {
    return {
      bgImg: '',
      bgbackup: '',
      opacity: 0.5,
      mtimer: null
    }
  },
  watch: {
    "$route.path"() {
      let wrapper = document.getElementsByClassName(
        'theme-vdoing-wrapper'
      )
      if (this.$frontmatter.custombg) {
        // 自定义背景图片
        if (this.bgbackup == '') {
          this.bgbackup = this.bgImg
        }
        this.bgImg = this.$frontmatter.custombg
        if (wrapper[0]) {
          wrapper[0].style.opacity = 0.8
        }
      } else {
        if (wrapper[0]) {
          wrapper[0].style.opacity = 1
        }
        if (this.bgbackup !== '') {
          this.bgImg = this.bgbackup
          this.bgbackup =''
        }
      }
    },
  },
  mounted () {
    let { bodyBgImg, bodyBgImgOpacity } = this.$themeConfig

    if (type(bodyBgImg) === 'string') {
      this.bgImg = bodyBgImg
    } else if (type(bodyBgImg) === 'array') {
      let count = 0

      this.bgImg = bodyBgImg[count]
      clearInterval(this.mtimer)
      this.mtimer = setInterval(() => {
        if (this.$frontmatter.custombg) return
        if (++count >= bodyBgImg.length) {
          count = 0
        }
        this.bgImg = bodyBgImg[count]
      }, 15000);
    }

    if (this.$frontmatter.custombg) {
      // 自定义背景图片
      if (this.bgbackup == '') {
        this.bgbackup = this.bgImg
      }
      this.bgImg = this.$frontmatter.custombg
      document.getElementsByClassName(
        'theme-vdoing-wrapper'
      )[0].style.opacity = 0.8;
      return;
    }

    if (bodyBgImgOpacity !== undefined) {
      this.opacity = bodyBgImgOpacity
    }

  }
}
</script>

<style lang='stylus'>
.body-bg
  position fixed
  left 0
  top 0
  z-index -999999
  height 100vh
  width 100vw
  transition background 0.5s
</style>
