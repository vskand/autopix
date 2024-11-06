<template>
	<li :class="{ completed: props.task.completed }">
		<input
			type="checkbox"
			:checked="props.task.completed"
			@change="handleComplete(props.task.id)"
		/>
		<input
			v-model="toDoInput"
			type="text"
			:disabled="inputDisabled"
			@keyup="handleEdit(props.task.id)"
		/>
		<button
			type="button"
			@click="inputDisabled = !inputDisabled"
			:disabled="props.task.completed"
		>
			edit
		</button>
		<button
			type="button"
			@click="handleDelete(props.task.id)"
			:disabled="props.task.completed"
		>
			delete
		</button>
	</li>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['updateTasksList'])
const props = defineProps(['task'])

const toDoInput = ref(props.task.content)
const inputDisabled = ref(true)

const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || [])

const handleEdit = (id) => {
	tasks.value = tasks.value.map((task) => {
		if (task.id === id) {
			task.content = toDoInput.value
		}
		return task
	})

	localStorage.setItem('tasks', JSON.stringify(tasks.value))
}
const handleDelete = (id) => {
	tasks.value = tasks.value.filter((task) => task.id !== id)

	localStorage.setItem('tasks', JSON.stringify(tasks.value))
	emit('updateTasksList')
}
const handleComplete = (id) => {
	tasks.value = tasks.value.map((task) => {
		if (task.id === id) {
			task.completed = !task.completed
		}
		return task
	})

	localStorage.setItem('tasks', JSON.stringify(tasks.value))
	emit('updateTasksList')
}
</script>

<style scoped>
li {
	display: flex;
	justify-content: center;
	gap: 1rem;
	margin-bottom: 0.5rem;
}
.completed {
	opacity: 0.5;
}
</style>
