<template>
  <div :class="wrapperClass">
    <p class="__gallery-card">
      123
    </p>
  </div>
</template>

<script>
export default {
  props: [
    "wrapperClass",
    "cardMeta",
    "cardData",
  ],
  created () {
    console.log("gallery-card created !")
  },
}
</script>

<style>
.__gallery-card {
  margin: 0;
}
</style>
