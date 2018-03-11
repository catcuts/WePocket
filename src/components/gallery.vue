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
      touchmoveZero: null,
      touchmoveOffset: null,
      touchmoveOffsetLast: null,
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
      // var left = this.currentCardIndex === 0 ? 
      //   (this.touchmoveOffset < 0 ?
      //     this.touchmoveOffset : 0
      //   ) 
      //   : 
      //   (this.currentCardIndex < this.meta.cards.length - 1 ?
      //     this.touchmoveOffset : 100 * (this.meta.cards.length - 1)
      //   )
      var left = 0
      if (
          (this.currentCardIndex > 0) && 
          (this.currentCardIndex < this.meta.cards.length - 1)
        ) {
        left = this.touchmoveOffset
        this.touchmoveOffsetLast = left
      } else {
        left = this.touchmoveOffsetLast
      }
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
      this.touchmoveZero = $event.clientX
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
    },
    onTap ($event) {
      console.log("tapped")
      if (!this.tapLock) {
        this.shortTouched = false
      }
    },
    onTouchmove ($event) {
      // console.log("touch moving:", $event)
      clearTimeout(this.touchTimeout)
      this.touchmoveOffset = $event.clientX - this.touchmoveZero
      this.touchmoveZero = $event.clientX
      this.currentCardIndex += Math.round(2 * this.touchmoveOffset / wx.getSystemInfoSync().windowWidth)
      console.log("touch moving:", this.currentCardIndex)
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
