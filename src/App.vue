<template>
  <div id="app">
    <header class="teal darken-2 center-align">
      <h1>To Do List</h1>
    </header>
    <main>
      <div class="container">
        <input
          type="text"
          v-model="newTask.title"
          placeholder="Add new task"
          class="new-task-input"
        />
        <button @click="addTask" class="add-btn waves-effect waves-light btn-small">
          <i class="material-icons">Add task</i> </button>
        <ul class="collection">
          <li v-for="task in tasks" :key="task.id" class="collection-item">
            <label>
              <input
                type="checkbox"
                v-model="task.completed"
                @change="toggleComplete(task)"
              />
              <span :class="{ completed: task.completed }">{{ task.title }}</span>
            </label>
            <a @click="deleteTask(task)" class="secondary-content">
              <i class="material-icons red-text text-darken-4">delete</i>
            </a>
          </li>
        </ul>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: {
        id: 0,
        title: "",
        completed: false,
      },
    };
  },
  methods: {
    addTask() {
      if (this.newTask.title.trim()) {
        this.newTask.id = Date.now();
        this.tasks.push({ ...this.newTask }); 
        this.newTask.title = ""; 
      }
    },
    toggleComplete(task) {
      task.completed = !task.completed;
    },
    deleteTask(task) {
      const index = this.tasks.findIndex((t) => t.id === task.id);
      if (index > -1) {
        this.tasks.splice(index, 1);
      }
    },
  },

  mounted() {
    const localTodos = localStorage.getItem("todos");
    if (localTodos) {
      this.tasks = JSON.parse(localTodos);
    }
  },

  beforeDestroy() {
    
    localStorage.setItem("todos", JSON.stringify(this.tasks));
  },
};
</script>

<style scoped>
body {
  font-family: Arial, sans-serif;
}

header {
  padding: 60px;
  color: #f6f3f3;
}

.container {
  width: 500px;
  margin: 0 auto;
  padding: 20px;
}

.new-task-input {
  width: 70%;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  margin-bottom: 10px;
}

.add-btn {
  margin-left: 10px;
}

.collection {
  list-style: none;
  padding: 0;
}

.collection-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #ddd;
}

.completed {
  text-decoration: line-through;
  opacity: 0.7;
}

.secondary-content {
  cursor: pointer;
}

.secondary-content i {
  font-size: 20px;
}
</style>
