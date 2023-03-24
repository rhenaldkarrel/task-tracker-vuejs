<template>
  <AddTask @add-task="addTask" v-if="isAddTask" />
		<Tasks
			@delete-task="onDelete"
			@toggle-reminder="toggleReminder"
			:tasks="state.tasks"
		/>
</template>

<script setup>
import AddTask from '../components/AddTask.vue';
import Tasks from '../components/Tasks.vue';
import { onMounted, reactive } from 'vue';

const props = defineProps({
  isAddTask: Boolean,
})

const state = reactive({
	tasks: [],
});

onMounted(async () => {
	state.tasks = await getTasks();
});

const getTasks = async () => {
	const res = await fetch('api/tasks');
	const data = res.json();
	return data;
};

const getTaskById = async (id) => {
	const res = await fetch(`api/tasks/${id}`);
	const data = await res.json();
	return data;
};

const onDelete = async (id) => {
	if (confirm('Are you sure?')) {
		const res = await fetch(`api/tasks/${id}`, {
			method: 'DELETE',
		});

		if (res.status === 200) {
			state.tasks = state.tasks.filter((task) => task.id !== id);
		} else {
			alert('Error deleting task!');
		}
	}
};

const toggleReminder = async (id) => {
  const taskToToggle = await getTaskById(id);
  const updatedTask = { ...taskToToggle, reminder: !taskToToggle.reminder };

  const res = await fetch(`/api/tasks/${id}`, {
    method: 'PUT',
    headers: {
      'Content-type': 'application/json',
    },
    body: JSON.stringify(updatedTask),
  })
  const data = await res.json();

	state.tasks = state.tasks.map((task) =>
		task.id === id ? { ...task, reminder: data.reminder } : task
	);
};

const addTask = async (newTask) => {
	const res = await fetch('api/tasks', {
		method: 'POST',
		headers: {
			'Content-type': 'application/json',
		},
		body: JSON.stringify(newTask),
	});

	const data = await res.json();

	state.tasks = [...state.tasks, data];
  state.toggleAddTask = false;
};
</script>