<template>
  <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>
      {{ task.text }}
      <i @click="deleteTask(task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>
  </div>

</template>

<script>
export default {
  name : "TaskItem",
  props : {
    task : Object
  },
  methods : {
    deleteTask(id) {
      this.$emit('delete-task', id)
    }
  }
}
</script>

<style scoped>
.fas {
  color: red;
}

.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}

.task.reminder {
  border-left: 5px solid green;
}

.task h3 {
  font-weight: 600;
  letter-spacing: 0.6px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
