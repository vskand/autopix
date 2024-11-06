<template>
	<div class="container">
		<h2>To Do List</h2>

		<div>
			<label for="new-task">Add new task</label>
			<br />
			<input
				id="new-task"
				v-model="newItemContent"
				type="text"
				v-on:keyup.enter="createNewTask"
			/>

			<ul>
				<ToDoItem
					v-for="task in tasks"
					:key="task.id"
					:task="task"
					@updateTasksList="populateTasks"
				/>
			</ul>
		</div>
	</div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

import ToDoItem from './ToDoItem.vue'

const tasks = ref(JSON.parse(localStorage.getItem('tasks')) || [])

const newItemContent = ref('')

const createNewTask = () => {
	const existingTasks = JSON.parse(localStorage.getItem('tasks')) || {}
	const newTask = {
		id: Date.now(),
		content: newItemContent.value,
		completed: false,
	}

	existingTasks.push(newTask)
	tasks.value = existingTasks
	localStorage.setItem('tasks', JSON.stringify(tasks.value))
	newItemContent.value = ''
}

const populateTasks = () => {
	tasks.value = JSON.parse(localStorage.getItem('tasks'))
}
</script>

<style scoped>
ul {
	padding: 0;
	margin-top: 1rem;
}
</style>
