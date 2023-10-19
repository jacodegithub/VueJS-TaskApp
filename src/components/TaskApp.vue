<template>
    <section>
            <div class="task-app">
                <h1>Task List</h1>
                <div class="task-input-section">
                    <label>Task Title</label>
                    <input type="text" v-model="taskTitle" placeholder="enter title" />
                    <button @click="addTask">Add Task</button>
                </div>

                <select v-model="selectedCompletionStatus">
                    <option value="all">All</option>
                    <option value="completed">Completed</option>
                    <option value="incomplete">Incomplete</option>
                </select>

                <div class="task-display-section">
                    <h2>Tasks</h2>
                    <table class="list-section" v-for="task in tasks" :key="task.id">
                        <td @click="markTaskComplete(task)">{{ task.completed ? 'Completed' : 'Incomplete' }}</td>
                        <td>
                            {{ task.title }} {{ task.id }}
                        </td>
                        <button @click="deleteTask(task)">Delete</button>
                    </table>
                </div>
            </div>
    </section>
</template>
  
<script>
import { defineComponent } from 'vue';

const TaskApp = defineComponent({
    name: 'App',
    data() {
        return {
            taskTitle: '',
            tasks: [],
            selectedCompletionStatus: 'all',
        };
    },
    methods: {
        addTask() {
            if (this.taskTitle) {
                this.tasks.push({
                    id: Math.random().toString(36).substring(7),
                    title: this.taskTitle,
                    completed: false,
                });
                this.taskTitle = '';
            } else {
                alert('Please enter a task title.');
            }
        },
        deleteTask(task) {
            this.tasks = this.tasks.filter(t => t.id !== task.id);
            this.$forceUpdate();
        },
        filteredTasks() {
            // Filter the tasks based on the selected completion status
            return this.tasks.filter(task => {
                switch (this.selectedCompletionStatus) {
                case 'all':
                    return true;
                case 'completed':
                    return task.completed;
                case 'incomplete':
                    return !task.completed;
                default:
                    return true;
                }
            });
        },
        markTaskComplete(task) {
            task.completed = !task.completed
        }
    },
});

export default TaskApp;
</script>
  
<style>
section {
    display: block;
}

h1 {
    text-transform: uppercase;
    font-size: 2rem;
    font-weight: 600;
}

.task-app {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.task-input-section {
    display: flex;
    gap: 1rem;
}

.task-input-section label {
    font-size: 1.3rem;
    text-transform: lowercase;
}

.task-input-section input {
    border: 0;
    width: 15rem;
    height: 2.5rem;
    font-size: 1rem;
    border-radius: .3rem;
    outline: none;
}
.task-input-section button {
    border: 0;
    text-transform: uppercase;
    font-size: .8rem;
    font-weight: bold;
    border-radius: .3rem;
    cursor: pointer;
    background: black;
    color: whitesmoke;
}

.task-input-section button:active {
    transform: scale(.95);
    background: rgba(0, 0, 0, 0.4);
}

.task-display-section {
    display: flex;
    align-items:flex-start;
    justify-content: flex-start;
    flex-direction: column;
}

.list-section {
    display: flex;
    align-items: center;
    gap: 1rem;
    text-decoration: none;
    list-style-type: none;
    margin-block-start: 0;
    margin-block-end: 0;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    padding-inline-start: 00px;
}

select {
    margin-top: 1rem;
    margin-bottom: 1rem;
    outline: none;
    width: 6rem;
    height: 1.5rem;
    border-radius: .8rem;
}
</style>
  