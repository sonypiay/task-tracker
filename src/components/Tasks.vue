<template>
    <div v-if="tasks.length == 0" class="empty-list">
        No task was added.
    </div>
    <div v-else v-for="task in tasks" :key="task.id">
        <Task 
        :task="task"
        @delete-task="$emit('delete-task', task.id)"
        @toggle-reminder="$emit('toggle-reminder', task.id)" />
    </div>
</template>

<script>
import Task from '@/components/Task.vue';

export default {
    name: 'Tasks',
    props: {
        tasks: Array,
    },
    components: {
        Task
    },
    emits: ['delete-task', 'toggle-reminder'],
}
</script>

<style scoped>
.empty-list {
    padding: 1rem;
    text-align: center;
    background: steelblue;
    color: #ffffff;
    border-radius: 5px;
}
</style>