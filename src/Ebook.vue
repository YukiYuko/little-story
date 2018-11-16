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
      <FootBar v-show="show"></FootBar>
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
      show: false
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
      console.log(this.book)
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
