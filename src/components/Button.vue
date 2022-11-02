<template>
  <button @click="onClick()" :style="{backgroundColor: color}" class="btn">{{ text }}</button>
</template>

<script>
export default {
  name : "ButtonDefault",
  props : {
    text : String,
    color : String,
  },
  methods : {
    onClick () {
      console.log( "Clicked item" )
    }
  }
}
</script>

<style scoped>

</style>
