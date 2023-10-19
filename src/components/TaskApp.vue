<template>
    <section>
            <div class="task-app">
                <h1>Task List</h1>
                <div class="task-input-section">
                    <label>Task</label>
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
                    <table class="list-section">
                        <thead>
                            <tr>
                                <th>Task</th>
                                <th>Status</th>
                                <th>Actions</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="task in filteredTasks" :key="task.id">
                                <td>{{ task.title }}</td>
                                <td>
                                    <button class="mark-button" @click="markTaskComplete(task)">
                                    {{ task.completed ? 'Completed' : 'Incomplete' }}
                                    </button>
                                </td>
                                <td><button class="delete-btn" @click="deleteTask(task)">Delete</button></td>
                            </tr>
                        </tbody>
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
    computed: {
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
        markTaskComplete(task) {
            task.completed = !task.completed
        }
    },
});

export default TaskApp;
</script>
  
<style>
body {
    background-color: aliceblue;
    color: black;
}
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
    box-shadow: -0px 5px 5px 0px grey;
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
    align-items: center;
    justify-content: center;
    flex-direction: column;
}

.list-section {
    border-collapse: collapse;
    width: 100%;
    gap: 1rem;
    text-decoration: none;
    list-style-type: none;
    margin-block-start: 0;
    margin-block-end: 0;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    padding-inline-start: 00px;
}


.list-section th,
.list-section td {
  /* border: 1px solid black; */
  padding: 10px 20px;
}

select {
    margin-top: 1rem;
    margin-bottom: 1rem;
    outline: none;
    width: 6rem;
    height: 1.5rem;
    border-radius: .8rem;
}

.mark-button {
    border: none;
    outline: none;
    background: transparent;
    font-size: .6rem;
    font-weight: bold;
    text-transform: uppercase;
}

.delete-btn {
    border: none;
    outline: none;
    background: rgba(255, 0, 0, 0.5);
    padding: .5rem 1rem;
    border-radius: .3rem;
    font-weight: bold;
}
.delete-btn:active {
    background: rgba(255, 0, 0, 0.8);
    transform: scale(.9);
}
th {
    font-size: 1rem;
    font-weight: bold;
}
h2 {
    text-align: center;
}
</style>
  