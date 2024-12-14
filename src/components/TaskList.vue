<template>
  <div class="task-manager">
    <h1>Task Manager</h1>

    <button @click="toggleTaskForm">
      {{ showTaskForm ? "Hide" : "Add New Task" }}
    </button>

    <div v-if="showTaskForm" class="task-form">
      <input
        v-model="newTask"
        placeholder="Enter task name (min 3 chars)"
        @keydown.enter="addTask"
      />
      <button @click="addTask">Add Task</button>
    </div>

    <p v-if="tasks.length === 0">No tasks available</p>

    <ul>
      <li
        v-for="task in tasks"
        :key="task.id"
        :class="{ completed: task.completed }"
      >
        <span @click="toggleTaskCompletion(task)">{{ task.name }}</span>
        <button @click="deleteTask(task.id)">Delete</button>
      </li>
    </ul>

    <button v-if="completedTasks.length > 0" @click="filterCompletedTasks">
      Show Completed Tasks
    </button>
  </div>
</template>

<script>
import { reactive, computed, ref, watch } from "vue";

export default {
  name: "TaskList",
  setup() {
    const tasks = reactive(JSON.parse(localStorage.getItem("tasks")) || []);
    const newTask = ref("");
    const showTaskForm = ref(false);
    const completedTasks = computed(() =>
      tasks.filter((task) => task.completed)
    );

    const toggleTaskForm = () => {
      showTaskForm.value = !showTaskForm.value;
    };

    const addTask = () => {
      if (newTask.value.trim().length < 3) {
        alert("Task name must have at least 3 characters.");
        return;
      }
      tasks.push({ id: Date.now(), name: newTask.value, completed: false });
      newTask.value = "";
    };

    const toggleTaskCompletion = (task) => {
      task.completed = !task.completed;
    };

    const deleteTask = (id) => {
      const index = tasks.findIndex((task) => task.id === id);
      if (index !== -1) tasks.splice(index, 1);
    };

    const filterCompletedTasks = () => {
      alert(
        `Completed Tasks: ${completedTasks.value.map((t) => t.name).join(", ")}`
      );
    };

    watch(
      () => tasks,
      (newTasks) => {
        localStorage.setItem("tasks", JSON.stringify(newTasks));
      },
      { deep: true }
    );

    return {
      tasks,
      newTask,
      showTaskForm,
      completedTasks,
      toggleTaskForm,
      addTask,
      toggleTaskCompletion,
      deleteTask,
      filterCompletedTasks,
    };
  },
};
</script>

<!-- <style scoped>
.task-manager {
  max-width: 600px;
  margin: 40px auto;
  font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  color: #333;
  background-color: #fefefe;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #007bff;
}

.task-form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px 0;
}

.task-form input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  color: #333;
  outline: none;
  transition: border-color 0.2s;
}

.task-form input:focus {
  border-color: #007bff;
}

.task-form button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.task-form button:hover {
  background-color: #218838;
  transform: scale(1.05);
}

ul {
  list-style: none;
  padding: 0;
  margin: 20px 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 10px 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

li:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

li.completed {
  background-color: #d4edda;
  border-color: #c3e6cb;
  text-decoration: line-through;
  color: #155724;
}

li button {
  padding: 5px 15px;
  font-size: 14px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

li button:hover {
  background-color: #c82333;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

button:hover {
  transform: scale(1.05);
}

button {
  background-color: #007bff;
  color: white;
}

button:hover {
  background-color: #0056b3;
}
</style> -->


<!-- <style scoped>
.task-manager {
  max-width: 600px;
  margin: 40px auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  color: #333;
  background-color: #fefefe;
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

h1 {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #007bff;
}

.task-form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px 0;
}

.task-form input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  color: #333; /* Text color */
  background-color: #f4f8fb; /* Light blue background */
  outline: none;
  transition: border-color 0.2s, background-color 0.3s;
}

.task-form input:focus {
  border-color: #007bff;
  background-color: #eaf4ff; /* Slightly brighter blue on focus */
}

.task-form button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.task-form button:hover {
  background-color: #218838;
  transform: scale(1.05);
}

ul {
  list-style: none;
  padding: 0;
  margin: 20px 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 10px 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

li:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

li.completed {
  background-color: #d4edda;
  border-color: #c3e6cb;
  text-decoration: line-through;
  color: #155724;
}

li button {
  padding: 5px 15px;
  font-size: 14px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

li button:hover {
  background-color: #c82333;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

button {
  background-color: #007bff;
  color: white;
}

button:hover {
  background-color: #0056b3;
}
</style> -->


<style scoped>
.task-manager {
  max-width: 600px;
  margin: 40px auto;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  text-align: center;
  color: #333;
  background: linear-gradient(135deg, #f5f7fa, #c3dafe); /* Light gradient background */
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

body {
  margin: 0;
  padding: 0;
  background: linear-gradient(135deg, #89d4cf, #6e45e2); /* Soft gradient background for entire page */
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

h1 {
  font-size: 2rem;
  margin-bottom: 20px;
  color: #007bff;
}

.task-form {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
  margin: 20px 0;
}

.task-form input {
  flex: 1;
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  color: #333;
  background-color: #f4f8fb;
  outline: none;
  transition: border-color 0.2s, background-color 0.3s;
}

.task-form input:focus {
  border-color: #007bff;
  background-color: #eaf4ff;
}

.task-form button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #28a745;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

.task-form button:hover {
  background-color: #218838;
  transform: scale(1.05);
}

ul {
  list-style: none;
  padding: 0;
  margin: 20px 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 10px 0;
  padding: 10px 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f9f9f9;
  transition: background-color 0.3s, transform 0.2s, box-shadow 0.3s;
}

li:hover {
  transform: scale(1.02);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

li.completed {
  background-color: #d4edda;
  border-color: #c3e6cb;
  text-decoration: line-through;
  color: #155724;
}

li button {
  padding: 5px 15px;
  font-size: 14px;
  background-color: #dc3545;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

li button:hover {
  background-color: #c82333;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, transform 0.2s;
}

button {
  background-color: #007bff;
  color: white;
}

button:hover {
  background-color: #0056b3;
}
</style>
