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
        data:{ img: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1520880640853&di=539dd8329db3a0fcb23cd66a80442bda&imgtype=0&src=http%3A%2F%2Fimg5.duitang.com%2Fuploads%2Fitem%2F201411%2F18%2F20141118230656_WKeu8.jpeg" }
      },
      {
        meta:{},
        data:{ img: "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1520880678186&di=569f29a975d866e6a582bbd3a5d4366a&imgtype=0&src=http%3A%2F%2Fimg5.duitang.com%2Fuploads%2Fitem%2F201410%2F27%2F20141027232616_UuR4Y.jpeg" }
      },
      {
        meta:{},
        data:{ img: "https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=3095130321,545183901&fm=27&gp=0.jpg" }
      },
      {
        meta:{},
        data:{ img: "https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=39342484,4111561145&fm=27&gp=0.jpg" }
      },
    ]
  }
}

</script>
<style>
page, .index_wrapper {
  height: 100vh;
  width: auto;
  overflow: hidden;
}
</style>
