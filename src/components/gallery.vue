<template>
  <div class="__gallery_wrapper"
      :class="__wrapperClass"
      :style="__wrapperStyle"
      @touchstart="onTouchstart" 
      @touchend="onTouchend" 
      @touchcancel="onTouchcancel"
      @tap="onTap"
      @touchmove="onTouchmove">
    <card
      v-for="(card, index) in cards"
      :key="index"
      :wrapper-class="index"
      :wrapper-style="cardWrapperClass"
      :card-meta="card.meta"
      :card-data="card.data">
    </card>
  </div>
</template>

<script>
import card from '@/components/card'

var windowWidth = wx.getSystemInfoSync().windowWidth
var half_windowWidth = windowWidth / 2

export default {
  components: {
    card,
  },
  props: [
    "wrapperClass",
    "wrapperStyle",
    "direction",
    "cards",
    "initCardIndex"
  ],
  data () {
    return {
      touchTimeout: null,
      shortTouched: false,
      tapLock: false,
      touchStartTime: 0,

      // finger
      fingerStart: 0,
      fingerLast: 0,
      LstOffsetFromStart: 0,  
      
      // gallery
      leftOffset: 0,
      currentCardIndex: 0,
    }
  },
  computed: {
    meta () {
      return {
        wrapperClass: this.wrapperClass || "",
        direction: this.direction || "vertical",
        cards: this.cards || [],
        initCardIndex: this.initCardIndex || 0,
      }
    },
    __wrapperClass () {
      return this.shortTouched ? 
        (this.meta.wrapperClass) + " short_touched" : 
        (this.meta.wrapperClass)
    },
    __wrapperStyle () {
      var width = 100 * this.meta.cards.length
      var left = this.leftOffset
      return `width: ${width}vw;\
              left: ${left}px;`
    },
    cardWrapperClass () {
      return ""
    }
  },
  methods: {
    onTouchstart ($event) {
      console.log("touch starts")
      this.tapLock = false
      this.fingerLast = this.fingerStart = $event.clientX
      this.LstOffsetFromStart = 0
      this.touchStartTime = new Date().getTime()
      this.touchTimeout = setTimeout(() => {
        this.shortTouched = true
        this.tapLock = true
        console.log("short touched")
      }, 300)
    },
    onTouchcancel ($event) {
      console.log("touch cancelled")
      clearTimeout(this.touchTimeout)
    },
    onTouchend ($event) {
      console.log("touch ends")
      clearTimeout(this.touchTimeout)
      if (this.shortTouched) {
        var touchEndTime = new Date().getTime()
        var fingerEnd = $event.clientX
        var velocity = (fingerEnd - this.fingerStart) / (touchEndTime - this.touchStartTime)
        this.currentCardIndex += velocity > 3 ? 1 : 0
        this.leftOffset = - this.currentCardIndex * windowWidth
      }
    },
    onTap ($event) {
      console.log("tapped")
      if (!this.tapLock) {
        this.shortTouched = false
      }
    },
    onTouchmove ($event) {
      clearTimeout(this.touchTimeout)
      var fingerCurrent = $event.clientX
      var offsetFromLast = fingerCurrent - this.fingerLast
      var CurOffsetFromStart = Math.abs(fingerCurrent - this.fingerStart)
      if (this.shortTouched && (offsetFromLast !== 0)) {

        this.leftOffset += offsetFromLast
         
        var needSwitchCard = false

        var curTime = new Date().getTime()
        var velocity = this.lstTime ? Math.abs(offsetFromLast) / (curTime - this.lstTime) : 0
        if (
          (this.LstOffsetFromStart < half_windowWidth
          && CurOffsetFromStart >= half_windowWidth)
          ||
          (this.LstOffsetFromStart >= half_windowWidth
          && CurOffsetFromStart < half_windowWidth)
          ) {
          needSwitchCard = true
        }

        if (needSwitchCard) {
          this.currentCardIndex += (fingerCurrent - this.fingerStart > 0 ? -1 : 1)
          this.currentCardIndex = Math.min(Math.max(0, this.currentCardIndex), this.meta.cards.length - 1)
        }
        
        // var info = {
        //   velocity: velocity,
        //   needSwitchCard: needSwitchCard,
        //   currentCardIndex: this.currentCardIndex
        // }

        this.lstTime = curTime
        this.fingerLast = fingerCurrent
        this.LstOffsetFromStart = CurOffsetFromStart

        // console.log("touch moving:", info)
      }
    },
  },
  created () {
    console.log("gallery created !")
  },
}
</script>

<style>

.__gallery_wrapper {
  margin: 0;
  height: 100vh;  /* (1) */
  background-color: grey;
  position: relative;
}

  .__gallery_wrapper > .__card_wrapper {
    animation: zoom_out 0.12s steps(50, end);
    height: 100vh;
    width: 100vw;
    display: inline-block;
    background-color: blue;
    transform: scale(1);
  }

  .__gallery_wrapper.short_touched > .__card_wrapper {
    animation: zoom_in 0.12s steps(50, end);
    transform: scale(0.8);
    border-radius: 10px;
    overflow: hidden;
  }

@keyframes zoom_in {
  from { transform: scale(1); }
}

@keyframes zoom_out {
  from { transform: scale(0.8); }
}

/* (1) 
    当 元素 height: 100%; 时, padding: x unit; 会把元素撑开 2x units
*/

/* (2)
    每个组件的最外层都有一个类, 类名格式为: __组件名_wrapper
*/

</style>
