<template>
  <header>
    <h1>{{ title }}</h1>
    <Button @click="btnOnClick" :text="showAddTask ? 'Close' : 'Add Task'"
            :color="showAddTask ? '#da2539' : '#1fc07b'" />
  </header>

</template>

<script>
import Button from "@/components/Button";

export default {
  name : "HeaderMain",
  props : {
    title : String,
    showAddTask: Boolean,
  },
  components : {
    Button,
  },
  methods: {
    btnOnClick() {
      this.$emit('btn-click')
    },
  },
}
</script>

<style scoped>

header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2em;
  letter-spacing: 1px;
}

</style>
