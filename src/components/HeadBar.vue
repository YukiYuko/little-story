<template>
  <div class="headBar">
    <div class="headBarBox" flex justify="between">
      <div class="left" flex items="center" justify="center"><i class="iconfont icon-fanhui"></i></div>
      <div class="right" flex items="center">
        <i class="iconfont icon-shujia"></i>
        <i class="iconfont icon-me"></i>
        <i class="iconfont icon-gengduo"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeadBar',
  data () {
    return {
    }
  }
}
</script>

<style lang="scss" scoped>
@import "../assets/styles/mixins";
.headBar{
  position: absolute; left: 0; top: 0; width: 100%; height: 0.9rem;
  @include drop-shadow-bottom; z-index: 100; background-color: #fff;
  .headBarBox{
    height: 100%;
    .left, .right{
      height: 100%;
    }
    .left{
      width: 1rem;
    }
    .right{
      i{ margin-right: 0.3rem }
    }
  }
}
</style>
