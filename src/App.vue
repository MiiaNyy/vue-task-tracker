<template>
  <div class="container">
    <Header @btn-click="toggleAddTask" title="Task Tracker" :showAddTask="showAddTask"/>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
  <Footer/>

  </div>

</template>

<script>
import Header from "@/components/Header";
import Tasks from "@/components/Tasks";
import AddTask from "@/components/AddTask";
import Footer from "@/components/Footer";

export default {
  name : 'App',
  components : {
    Header,
    Tasks,
    AddTask,
    Footer,
  },
  data () {
    return {
      tasks : [],
      showAddTask : false,
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

    toggleAddTask () {
      this.showAddTask = !this.showAddTask;
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

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}


html, body {
  font-family: 'Poppins', sans-serif;

}

.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}

.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}

.btn:focus {
  outline: none;
}

.btn:active {
  transform: scale(0.98);
}

.btn-block {
  display: block;
  width: 100%;
}
</style>
