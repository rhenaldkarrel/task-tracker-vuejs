<script setup>
import { onMounted, reactive } from 'vue';
import AddTask from './components/AddTask.vue';
import Header from './components/Header.vue';
import Tasks from './components/Tasks.vue';

const state = reactive({
	tasks: [],
  toggleAddTask: false,
});

onMounted(() => {
	state.tasks = [
		{
			id: '1',
			text: 'Doctors Appointment',
			day: 'March 5th at 2:30pm',
			reminder: true,
		},
		{
			id: '2',
			text: 'Meeting with boss',
			day: 'March 6th at 1:30pm',
			reminder: true,
		},
		{
			id: '3',
			text: 'Food shopping',
			day: 'March 7th at 2:00pm',
			reminder: false,
		},
	];
});

const onDelete = (id) => {
	if (confirm('Are you sure?')) {
		const prevTasks = state.tasks;
		state.tasks = prevTasks.filter((task) => task.id !== id);
	}
};

const toggleReminder = (id) => {
	const prevTasks = state.tasks;
	state.tasks = prevTasks.map((task) =>
		task.id === id ? { ...task, reminder: !task.reminder } : task
	);
};

const addTask = (newTask) => state.tasks.push(newTask);

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
