<template>
  <div class="gallery_wrap" :class="shortTouchedClass"
    @touchstart="onTouchstart" 
    @touchend="onTouchend" 
    @touchcancel="onTouchcancel"
    @tap="onTap">
    <gallery-card
      v-for="(card, index) in cards"
      :key="index"
      :wrapper-class="['gallery_card', index]"
      :card-meta="card.meta"
      :card-data="card.data">
    </gallery-card>
  </div>
</template>

<script>
// Use Vuex
import store from './store'
import galleryCard from '@/components/gallery-card'

export default {
  components: {
    galleryCard,
  },
  data () {
    return {
      touchTimeout: null,
      shortTouched: false,
      tapLock: false,
      cards: [],
    }
  },
  computed: {
    shortTouchedClass () {
      return this.shortTouched ? 'short_touched' : ''
    }
  },
  methods: {
    onTouchstart ($event) {
      console.log($event)
      this.tapLock = false
      this.touchTimeout = setTimeout(() => {
        console.log("sunken !")
        this.shortTouched = true
        this.tapLock = true
      }, 300)
    },
    onTouchcancel ($event) {
      clearTimeout(this.touchTimeout)
    },
    onTouchend ($event) {
      clearTimeout(this.touchTimeout)
    },
    onTap ($event) {
      if (!this.tapLock) {
        this.shortTouched = false
      }
    },
  },
  created () {
    console.log("index created !")
    this.cards = [
      {
        meta:{},
        data:{}
      },
    ]
  }
}

</script>
<style>
page {
  height: 100%;
  width: auto;
  /*overflow: hidden;*/
}

.gallery_wrap {
  margin: 0;
  height: 100%;  /* (1) */
  background-color: grey;
}

  .gallery_wrap > .gallery_card {
    height: 100%;
    background-color: blue;
    transform: scale(1);
  }

  .gallery_wrap.short_touched > .gallery_card {
    animation: zoom_in 0.6s steps(30, end);
    transform: scale(0.9);
  }

@keyframes zoom_in {
  from { transform: scale(1); }
}

@keyframes zoom_out {
  from { transform: initial; }
}

/* (1) 
    当 元素 height: 100%; 时, padding: x unit; 会把元素撑开 2x units.
*/

</style>
