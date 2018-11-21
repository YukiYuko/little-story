<template>
  <div class="footBar">
    <!--字体设置-->
    <transition name="slide-up">
      <div class="font-setting" v-show="show_font">
        <div class="font-setting-warp" flex justify="between" items="center">
          <div class="left">
            <i class="iconfont icon-down"></i>
          </div>
          <div class="center" box="1" flex>
            <div class="select-font" flex items="center" v-for="item in fonts"
                 :key="item" @click="setFontSize(item)">
              <span></span>
              <div class="circle" v-show="defaultFontSize === item"></div>
            </div>
          </div>
          <div class="right">
            <i class="iconfont icon-up"></i>
          </div>
        </div>
      </div>
    </transition>
    <!--主题设置-->
    <transition name="slide-up">
      <div class="themes" flex items="center" v-show="show_themes">
        <div @click="setTheme(item.name)" :class="[{active: defaultTheme === item.name},item.name]" box="1" v-for="(item, index) in themeList" :key="index">
          <a></a>
          <p>{{item.name}}</p>
        </div>
      </div>
    </transition>
    <!--进度设置-->
    <transition name="slide-up">
      <div class="slider" v-show="show_slide">
        <div class="slider-box">
          <label for="slider">
            <input type="range" id="slider"
                   name="slider"
                   class="ne-range"
                   :value="progress"
                   @change="onProgressChange($event.target.value)"
                   @input="onProgressInput($event.target.value)"
                   :style="{backgroundSize: `${progress}% 100%`}"
            />
          </label>
        </div>
        <div class="slide-num">{{bookAvailable ? progress + '%' : '加载中...'}}</div>
      </div>
    </transition>
    <!--跳转目录-->
    <transition name="slide-left">
      <div v-show="show_nav" class="navigation">
        <div class="navigation-item" :class="{active: href_url === item.href}" flex items="center" @click="jumpTo(item.href)"
             v-for="(item, index) in navigation" :key="index">
          {{index + '. ' +item.href}}
        </div>
      </div>
    </transition>
    <!--背景-->
    <transition name="fade">
      <div v-show="show_nav" @click="show_nav_fun" class="dark-shadow"></div>
    </transition>
    <div class="footBarBox" flex justify="between" items="center">
      <i @click="show_nav_fun" class="iconfont icon-mulu"></i>
      <i @click="show_slide_fun" class="iconfont icon-jindutiao"></i>
      <i @click="show_themes_fun" class="iconfont icon-liangdu"></i>
      <i @click="show_font_fun" class="iconfont icon-ziti"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: 'footBar',
  data () {
    return {
      fonts: [12, 14, 16, 18, 20],
      show_font: false,
      show_themes: false,
      show_slide: false,
      show_nav: false,
      progress: 0
    }
  },
  props: {
    defaultFontSize: Number,
    show: Boolean,
    themeList: Array,
    defaultTheme: String,
    bookAvailable: Boolean,
    navigation: Array,
    href_url: String
  },
  watch: {
    show: function (val) {
      if (!val) {
        this.show_font = false
      }
    }
  },
  methods: {
    show_font_fun () {
      this.show_font = !this.show_font
      this.show_themes = false
    },
    show_themes_fun () {
      this.show_themes = !this.show_themes
      this.show_font = false
    },
    show_slide_fun () {
      this.show_slide = !this.show_slide
    },
    show_nav_fun () {
      this.show_nav = !this.show_nav
    },
    setFontSize (item) {
      this.$emit('setFontSize', item)
    },
    setTheme (item) {
      this.$emit('setTheme', item)
    },
    jumpTo (href) {
      this.$emit('jumpTo', href)
      this.show_nav = false
    },
    onProgressChange (value) {
      this.$emit('onProgressChange', value)
      console.log(value)
    },
    onProgressInput (value) {
      this.progress = value
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/mixins";
.footBar{
  position: absolute; left: 0; bottom: 0; width: 100%; height: 0.9rem;
  @include drop-shadow-top; z-index: 100; background-color: #fff;
  .footBarBox{
    height: 100%; position: relative; background-color: #fff;
    i{
      width: 25%; font-size: 0.36rem;
    }
  }
  .font-setting{
    position: absolute; bottom: 0.9rem; left: 0; width: 100%;
    height: 1rem;
    @include drop-shadow-top; background-color: #fff;
    &:before{
      @include bottom-line;
    }
    .font-setting-warp{
      height: 100%; color: #999;
      .left, .right{
        width: 1rem; text-align: center;
      }
      .center{
        height: 100%; position: relative;
        .select-font{
          width: 20%; position: relative; height: 100%;
          &:first-child {
            justify-content: flex-end;
            &:before{
              display: none;
            }
          }
          &:last-child {
            &:after{
              display: none;
            }
          }
          &:before{
            content: '';
            width: 50%; height: 1px; background-color: #d6d6d6;
          }
          &:after{
            content: '';
            width: 50%; height: 1px; background-color: #d6d6d6;
          }
          span{
            width: 1px; height: 20%; background-color: #d6d6d6;
          }
          .circle{
            width: 16px; height: 16px; border-radius: 50%; background-color: #fff;
            border: 1px solid #cfcfcf;position: absolute;
            left: 50%; margin-left: -8px; box-shadow: 0 0 4px rgba(0,0,0,0.2);
            &:before{
              content: ''; width: 4px; height: 4px; background-color: #111;
              border-radius: 50%; position: absolute; left: 50%; top: 50%;
              transform: translate(-50%, -50%);
            }
          }
        }
      }
    }
  }
  /*主题*/
  .themes{
    min-height: 1rem; position: absolute; width: 100%; left: 0; bottom: 0.9rem;
    color: #999;
    @include drop-shadow-top; background-color: #fff;
    &:before{
      @include bottom-line;
    }
    a{
      height: 0.6rem; display: block;
    }
    p{
      padding: 0.1rem 0;
    }
    div{
      margin: 0 0.1rem;
      &.active{
        color: #111;
      }
      &.default{
        a{
          border: 1px solid #444;
        }
      }
      &.eye{
        a{
          background-color: aquamarine;
        }
      }
      &.night{
        a{
          background-color: #111;
        }
      }
      &.gold{
        a{
          background-color: antiquewhite;
        }
      }
    }
  }
  /*进度*/
  .slider{
    min-height: 1rem; position: absolute; width: 100%; left: 0; bottom: 0.9rem;
    color: #999;
    @include drop-shadow-top; background-color: #fff;
    &:before{
      @include bottom-line;
    }
    .slider-box{
      padding: 0 0.2rem;
    }
  }
  /*目录*/
  .navigation{
    width: 70%; position: fixed; left: 0; top: 0; bottom: 0;
    background-color: #fff; z-index: 999;padding: 0.4rem 0;
    overflow: auto;
    .navigation-item{
      height: 0.9rem; text-align: left;
      padding: 0 0.3rem; word-break: break-all;
      position: relative;
      &.active{
        color: #ff4242;
        &:after{
          @include bottom-line(#ff4242);
        }
      }
      &:after{
        @include bottom-line();
      }
    }
  }
  .dark-shadow{
    position: fixed; left: 0; right: 0; top: 0; bottom: 0;
    background-color: rgba(0,0,0,0.6);z-index: 998;
  }
}
/*说下关键点，thumb相关的是设置滑块按钮（即那个圆点）相关属性，track即那一条滑动条。*/

.ne-range_thumb,
input.ne-range[type=range]::-webkit-slider-thumb {
  width: 0.3rem;
  height: 0.3rem;
  border-radius: 50%;
  border: 0/**1px solid #45bd5c*/;
  background-color: #fff;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.21);
  -webkit-transition: border-color 0.15s, background-color 0.15s;
  transition: border-color 0.15s, background-color 0.15s;
  cursor: pointer;
  background-clip: padding-box;
  box-sizing: border-box;
}
.ne-range_track,
input.ne-range[type=range] {
  width: 100%;
  height: 4px;
  border-radius: 8px;
  margin: .8em 0;
  padding: 0;
  cursor: pointer;
  border: 0;
  /**background-color: #45bd5c;*/
  background: -webkit-linear-gradient(#797979, #797979) no-repeat #cccccc;
  background-size: 0% 100%;
}

/*Real Range*/
input.ne-range[type=range] {
  position: relative;
  outline: 0;
  -webkit-appearance: none !important;
}
input.ne-range[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none !important;
}
/*Virtual Range*/
/*
.ne-range {
  display: inline-block;
  position: relative;
  width: 100%;
  font-size: 1em;
}
.ne-range_thumb {
  position: absolute;
  top: 0;
  margin-left: -0.85em;
}
.ne-range_thumb.ondrag > .ne-range_tips {
  visibility: visible;
}
*/

</style>
