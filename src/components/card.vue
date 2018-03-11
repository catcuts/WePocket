<template>
  <div class="__card_wrapper"
      :class="wrapperClass"
      :style="wrapperStyle">
    <p>
        {{ cardData.text }}
    </p>
  </div>
</template>

<script>
export default {
  props: [
    "wrapperClass",
    "wrapperStyle",
    "cardMeta",
    "cardData",
  ],
  created () {
    console.log("card created !")
  },
}
</script>

<style scoped>
.__card_wrapper {
  padding: 10px;
}
</style>
