<template>
  <div class="index_wrapper">
    <gallery :cards="cards"></gallery>
  </div>
</template>

<script>
// Use Vuex
import store from './store'
import gallery from '@/components/gallery'

export default {
  components: {
    gallery,
  },
  data () {
    return {
      cards: [],
    }
  },
  computed: {
    
  },
  methods: {
    
  },
  created () {
    console.log("index created !")
    this.cards = [
      {
        meta:{},
        data:{ text: "meow" }
      },
      {
        meta:{},
        data:{ text: "wong" }
      },
    ]
  }
}

</script>
<style>
page, .index_wrapper {
  height: 100vh;
  width: auto;
  /*overflow: hidden;*/
}
</style>
