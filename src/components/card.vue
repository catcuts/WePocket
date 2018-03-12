<template>
  <div class="__card_wrapper"
      :class="wrapperClass"
      :style="wrapperStyle">
    <img style="height: 100vh; width: 100vw;" :src="cardData.img" />
  </div>
</template>

<script>
var windowHeight = wx.getSystemInfoSync().windowHeight
var windowWidth = wx.getSystemInfoSync().windowWidth

export default {
  props: [
    "wrapperClass",
    "wrapperStyle",
    "cardMeta",
    "cardData",
  ],
  data () {
    return {
      windowHeight: 504,
      windowWidth: windowWidth
    }
  },
  created () {
    console.log("card created !")
  },
}
</script>

<style>
.__card_wrapper {
  padding: 10px;
  border-radius: 5px;
}
</style>
