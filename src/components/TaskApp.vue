<template>
    <section>
            <div>
                <h1>Task List</h1>
                <div>
                    <label>Task Title</label>
                    <input type="text" v-model="taskTitle" placeholder="enter title" />
                    <button @click="addTask">Add Task</button>
                </div>
                <div>
                    <h2>Tasks</h2>
                    <div>
                        <ul v-for="task in tasks" :key="task.id">
                            <li>
                                {{ task.title }} {{ task.id }}
                                <button @click="deleteTask(task)">Delete</button>
                            </li>
                            <li>{{ task.completed ? 'Completed' : 'Incomplete' }}</li>
                        </ul>
                    </div>
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
            filterCompleted: false,
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
        filterTasks() {
            this.tasks = this.tasks.filter((task) => {
                return this.filterCompleted ? task.completed : true;
            });
        },
    },
});

export default TaskApp;
</script>
  
<style>

</style>
  