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
    defaultTasks()
    {
      this.tasks = [];
    },
    addTask(task)
    {
      this.tasks = [...this.tasks, task];
    },
    deleteTask(id)
    {
      if( confirm('Are you sure want to delete this task?') )
      {
        this.tasks  = this.tasks.filter( item => item.id !== id);
      }
    },
    toggleReminder(id)
    {
      this.tasks = this.tasks.map((item) => item.id === id ? {...item, reminder: !item.reminder} : item );
    },
    toggleAddTask()
    {
      this.showAddTask = !this.showAddTask;
    }
  },
  created() {
    this.defaultTasks();
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