<template>
  <div class="eBook">
    <transition name="slide-down">
      <HeadBar v-show="show"></HeadBar>
    </transition>
    <div class="eBook-warp">
      <div id="read"></div>
      <div class="mask" flex>
        <div class="left" @click="prev"></div>
        <div class="center" box="1" @click="open"></div>
        <div class="right" @click="next"></div>
      </div>
    </div>
    <transition name="slide-up">
      <FootBar v-show="show" :show="show"
               @setFontSize="setFontSize"
               @setTheme="setTheme"
               :themeList="themeList"
               :defaultFontSize="defaultFontSize"
               :defaultTheme="defaultTheme"></FootBar>
    </transition>
  </div>
</template>

<script>
import Epub from 'epubjs'
import HeadBar from '@/components/HeadBar'
import FootBar from '@/components/FootBar'
const DOWNLOAD_URL = '/static/book/64960.epub'
export default {
  name: 'Ebook',
  data () {
    return {
      show: false,
      defaultFontSize: 14,
      defaultTheme: 'default',
      themeList: [
        {
          'name': 'default',
          'style': {
            'body': {
              'color': '#000',
              'background': '#fff'
            }
          }
        },
        {
          'name': 'eye',
          'style': {
            'body': {
              'color': '#000',
              'background': '#ceeaba'
            }
          }
        },
        {
          'name': 'night',
          'style': {
            'body': {
              'color': '#fff',
              'background': '#000'
            }
          }
        },
        {
          'name': 'gold',
          'style': {
            'body': {
              'color': '#000',
              'background': 'rgb(241,236,226)'
            }
          }
        }
      ]
    }
  },
  components: {
    HeadBar,
    FootBar
  },
  methods: {
    // 电子书的解析和渲染
    showEpub () {
      // 生成book
      this.book = new Epub(DOWNLOAD_URL)
      // 生成Rendition 通过Book.renderTOo
      this.rendition = this.book.renderTo('read', {
        width: window.innerWidth,
        height: window.innerHeight
      })
      // 通过Rendtion.display 渲染电子书
      this.rendition.display()
      // 获取theme 对象
      this.themes = this.rendition.themes
      this.setFontSize(this.defaultFontSize)
      this.registerTheme()
      this.themes.select(this.defaultTheme)
    },
    // 设置字体
    setFontSize (fontSize) {
      this.defaultFontSize = fontSize
      this.themes && this.themes.fontSize(fontSize + 'px')
    },
    // 注册主题
    registerTheme () {
      this.themeList.forEach(({name, style}) => {
        this.themes.register(name, style)
      })
    },
    // 设置主题
    setTheme (name) {
      this.defaultTheme = name
      this.themes.select(this.defaultTheme)
    },
    // 上一页
    prev () {
      this.rendition.prev()
    },
    // 下一页
    next () {
      this.rendition.next()
    },
    // 打开上下菜单
    open () {
      this.show = !this.show
    }
  },
  mounted () {
    this.showEpub()
  }
}
</script>

<style lang="scss" scoped>
@import './assets/styles/mixins';
.eBook{
  position: relative;
  .eBook-warp{
    .mask{
      @include pos;
      .left{
        width: 2rem; height: 100%;
      }
      .center{
      }
      .right{
        width: 2rem; height: 100%;
      }
    }
  }
}
</style>
