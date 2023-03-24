<template>
	<form class="add-form" @submit="onSubmit">
		<div class="form-control">
			<label>Task</label>
			<input
				type="text"
				name="task"
				v-model="state.task"
				placeholder="Add Task"
			/>
		</div>
		<div class="form-control">
			<label>Day & Time</label>
			<input
				type="text"
				name="day"
				v-model="state.day"
				placeholder="Add Day & Time"
			/>
		</div>
		<div class="form-control form-control-check">
			<label>Set Reminder</label>
			<input type="checkbox" name="reminder" v-model="state.reminder" />
		</div>
		<input type="submit" value="Save Task" class="btn btn-block" />
	</form>
</template>

<script setup>
import { reactive } from 'vue';

const state = reactive({
	task: '',
	day: '',
	reminder: false,
});

const emits = defineEmits(['add-task']);

const onSubmit = (e) => {
	e.preventDefault();

	if (!state.task) {
		alert('Task cannot be null');
		return;
	}

	const newTask = {
		id: Math.floor(Math.random() * 100000),
		text: state.task,
		day: state.day,
		reminder: state.reminder,
	};

	emits('add-task', newTask);

	state.task = '';
	state.day = '';
	state.reminder = false;
};
</script>

<style scoped>
.add-form {
	margin-bottom: 40px;
}
.form-control {
	margin: 20px 0;
}
.form-control label {
	display: block;
}
.form-control input {
	width: 100%;
	height: 40px;
	margin: 5px;
	padding: 3px 7px;
	font-size: 17px;
}
.form-control-check {
	display: flex;
	align-items: center;
	justify-content: space-between;
}
.form-control-check label {
	flex: 1;
}
.form-control-check input {
	flex: 2;
	height: 20px;
}
</style>
