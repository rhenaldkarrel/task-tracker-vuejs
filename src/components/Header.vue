<script setup>
import { useRoute } from 'vue-router';
import Button from './Button.vue';
import { computed } from 'vue';

const route = useRoute();

const props = defineProps({
	title: {
		type: String,
		default: 'Task Tracker',
	},
	isAddTask: {
		type: Boolean,
		default: false,
	},
});

const emits = defineEmits(['toggle-add-task']);

const homePage = computed(() => {
  return route.path === '/' ? true : false;
})
</script>

<template>
	<header>
		<h1>{{ title }}</h1>
		<Button
      v-show="homePage"
			@btn-click="$emit('toggle-add-task')"
			:text="!isAddTask ? 'Add Task' : 'Cancel'"
			:color="!isAddTask ? 'green' : 'red'"
		/>
	</header>
</template>

<style scoped>
header {
	display: flex;
	justify-content: space-between;
	align-items: center;
	margin-bottom: 1.25rem;
}
</style>
