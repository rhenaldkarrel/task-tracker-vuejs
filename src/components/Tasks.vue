<script setup>
import TaskItem from './TaskItem.vue';

const props = defineProps({
	tasks: Array,
});

const emits = defineEmits(['delete-task', 'toggle-reminder']);
</script>

<template>
	<div v-for="task in tasks" :key="task.id">
		<TaskItem
			@toggle-reminder="$emit('toggle-reminder', task.id)"
			@delete-task="$emit('delete-task', task.id)"
			:task="task"
		/>
	</div>
</template>
