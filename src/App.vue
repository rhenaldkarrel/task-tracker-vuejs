<script setup>
import { onMounted, reactive } from 'vue';
import AddTask from './components/AddTask.vue';
import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';

const state = reactive({
	tasks: [],
	toggleAddTask: false,
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

const toggleAddTask = () => {
	state.toggleAddTask = !state.toggleAddTask;
};
</script>

<template>
	<div class="container">
		<Header @toggle-add-task="toggleAddTask" :isAddTask="state.toggleAddTask" />
		<AddTask @add-task="addTask" v-if="state.toggleAddTask" />
		<Tasks
			@delete-task="onDelete"
			@toggle-reminder="toggleReminder"
			:tasks="state.tasks"
		/>
	</div>
</template>

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
