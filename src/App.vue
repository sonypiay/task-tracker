<template>
  <div class="container">
    <!-- Heading -->
    <Header 
    @toggle-add-task="toggleAddTask" 
    :title="showAddTask ? titleAddTask : titleDefault"
    :showAddTask="showAddTask" />
    <!-- Heading -->

    <!-- Add Task -->
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <!-- Add Task -->

    <!-- List of Task -->
    <Tasks 
    :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder" />
    <!-- List of Task -->
  </div>
</template>

<script>
import Header from '@/components/Header';
import Tasks from '@/components/Tasks';
import AddTask from '@/components/AddTask';

export default {
  name: 'App',
  components: {
    Header,
    Tasks,
    AddTask
  },
  data() {
    return {
      tasks: [],
      showAddTask: false,
      titleAddTask: 'Add Task',
      titleDefault: 'Task Tracker',
    }
  },
  methods: {
    async getListTasks(id)
    {
      let url     = id 
      ? `api/tasks/${id}`
      : 'api/tasks';

      const res   = await fetch(url);

      if( res.status === 200 )
      {
        const data  = await res.json();
        this.tasks  = data;
      }
    },
    async addTask(task)
    {
      const res = await fetch('api/tasks', {
        method: 'post',
        headers: {
          'content-type': 'application/json',
        },
        body: JSON.stringify(task),
      });

      if( res.status !== 201 )
      {
        const data  = await res.json();
        this.tasks.push(data);
      }
      else
      {
        alert('There was an error when add new task.');
      }
    },
    async deleteTask(id)
    {
      if( confirm('Are you sure want to delete this task?') )
      {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'delete',
        });

        if( res.status === 200 )
        {
          this.tasks  = this.tasks.filter((item) => item.id !== id);
        }
        else
        {
          alert('There was an error when deleting task.');
        }
      }
    },
    async toggleReminder(id)
    {
      const getListTasks  = this.tasks.filter(item => item.id === id);
      const updateTask    = getListTasks.length === 0 ? null : getListTasks[0];

      if( updateTask ) updateTask.reminder = !updateTask.reminder;

      const res = await fetch(`api/tasks/${id}`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(updateTask)
      });

      if( res.status === 201 )
      {
        this.tasks  = this.tasks.map((item) => item.id === id ? {...item, reminder: updateTask.reminder} : item );
      }
    },
    toggleAddTask()
    {
      this.showAddTask = !this.showAddTask;
    }
  },
  mounted() {
    this.getListTasks();
  },
  emits: ['add-task', 'toggle-add-task'],
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
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