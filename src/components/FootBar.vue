<template>
  <div class="footBar">
    <transition name="slide-up">
      <div class="font-setting" v-show="show_font">
        <div class="font-setting-warp" flex justify="between" items="center">
          <div class="left">
            <i class="iconfont icon-down"></i>
          </div>
          <div class="center" box="1" flex>
            <div class="select-font" flex items="center" v-for="item in fonts"
                 :key="item" @click="currentFont = item">
              <span></span>
              <div class="circle" v-show="currentFont === item"></div>
            </div>
          </div>
          <div class="right">
            <i class="iconfont icon-up"></i>
          </div>
        </div>
      </div>
    </transition>
    <div class="footBarBox" flex justify="between" items="center">
      <i class="iconfont icon-mulu"></i>
      <i class="iconfont icon-jindutiao"></i>
      <i class="iconfont icon-liangdu"></i>
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
      currentFont: 14,
      show_font: false
    }
  },
  methods: {
    show_font_fun () {
      this.show_font = !this.show_font
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
    height: 100%;
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
            width: 12px; height: 12px; border-radius: 50%; background-color: #fff;
            border: 1px solid #cfcfcf;position: absolute;
            left: 50%; margin-left: -6px;
          }
        }
      }
    }
  }
}
</style>
