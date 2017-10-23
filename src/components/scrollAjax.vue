<template>
  <div class="scroll-ajax-wrap" ref="scrollWrap" v-finger:touch-move="touchMove" @touchstart="start"
       v-finger:touch-end="touchEnd" :style="wrapStyle">
    <header class="top-msg msg" :style="topStyle">
      <span>下拉刷新数据</span>
      <span>加载数据中...</span>
    </header>
    <main class="content-area">
      <slot></slot>
    </main>
    <footer class="bottom-msg msg" :style="bottomStyle">
      <span>上拉加载数据</span>
      <span>加载数据中...</span>
    </footer>
  </div>
</template>
<script>
  export default {
    name: 'myscrollAjax',
    data() {
      return {
        bottomStyle: {
          height: '60px'
        },
        wrapStyle: {
          paddingTop: '0px',
          transition: 'none'
        },
        topStyle: {
          height: '0px',
          transition: 'none'
        },
        startY: 0,
        startScroll: 0
      }
    },
    methods: {
      start(e) {
        this.startY = e.targetTouches[0].pageY
        this.startScroll = this.$el.scrollTop || 0;
      },
      touchMove(e) {
        if (!this.startScroll === 0) {
          return
        }
        let moverNum = e.targetTouches[0].pageY - this.startY - this.startScroll,
          top = this.topStyle,
          wrap = this.wrapStyle;
        top.height = wrap.paddingTop = moverNum > 0 ? moverNum + 'px' : '0px'
      },
      reset() {
//        let scrollElem = this.$refs.scrollWrap,
//          scrollHeight = scrollElem.scrollHeight,
//          offsetHeight = scrollElem.offsetHeight;
//        this.bottomStyle =
      },
      touchEnd(e) {
        let top = this.topStyle,
          wrap = this.wrapStyle,
          endX = e.changedTouches[0].pageX,
          endY = e.changedTouches[0].pageY,
          dy = this.startY - endY,
          dx = endX - this.startX;
        if (Math.abs(dx) < 2 && Math.abs(dy) < 2) {
          console.log("滑动距离太短")
          return;
        }
        top.transition = wrap.transition = '500ms'
        top.height = wrap.paddingTop = '0px'
        setTimeout(function () {
          top.transition = wrap.transition = 'none'
        }, 500)
      }
    }
  }
</script>
<style lang="less">
  @import './index.less';
</style>
