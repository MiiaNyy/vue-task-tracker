<template>
  <AddTask v-if="showAddTask" @add-task="addTask"/>
  <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import Tasks from "@/components/Tasks";
import AddTask from "@/components/AddTask";

export default {
  name : "HomePage",
  components : {
    Tasks, AddTask
  },
  props : {
    showAddTask : Boolean,
  },
  data () {
    return {
      tasks : [],
    }
  },
  methods : {
    async addTask (newTask) {
      const data = await this.postTask( newTask );
      this.tasks = [... this.tasks, data];

      this.toggleAddTask()
    },

    async deleteTask (id) {
      if ( confirm( `Are you sure you you want to delete item with id ${ id }` ) ) {
        const responseStatus = await this.deleteTaskFromDb( id )

        if ( responseStatus === 200 ) {
          this.tasks = this.tasks.filter( task => task.id !== id )
        } else {
          alert( 'Error while deleting task!' )
        }
      }
    },

    async toggleReminder (id) {
      const taskToToggle = await this.getTask( id );
      const updateTask = {
        ... taskToToggle,
        reminder : !taskToToggle.reminder
      };

      const data = await this.putTask( id, updateTask );

      this.tasks = this.tasks.map( task => {
        return task.id === id ? { ... task, reminder : data.reminder } : task
      } )
    },

    async getTasks () {
      const res = await fetch( 'api/tasks' );
      return res.json();
    },

    async getTask (id) {
      const res = await fetch( `api/tasks/${ id }` );
      return res.json();
    },

    async postTask (newTask) {
      const res = await fetch( 'api/tasks', {
        method : 'POST',
        headers : {
          'Content-type' : 'application/json',
        },
        body : JSON.stringify( newTask )

      } )
      return res.json();
    },

    async putTask (id, updateTask) {
      const res = await fetch( `api/tasks/${ id }`, {
        method : 'PUT',
        headers : {
          'Content-type' : 'application/json',
        },
        body : JSON.stringify( updateTask ),
      } )

      return res.json();
    },

    async deleteTaskFromDb (id) {
      const res = await fetch( `api/tasks/${ id }`, {
        method : 'DELETE'
      } )

      return res.status
    }
  },
  async created () {
    this.tasks = await this.getTasks();
  }

}
</script>

<style scoped>

</style>
