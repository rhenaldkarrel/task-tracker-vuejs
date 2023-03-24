<script setup>
const props = defineProps({
	text: String,
	color: String,
});

const emits = defineEmits(['btn-click']);

const onClick = () => {
  emits('btn-click')
}
</script>

<template>
	<button @click="onClick" :style="{ background: color }" class="btn">
		{{ text }}
	</button>
</template>
