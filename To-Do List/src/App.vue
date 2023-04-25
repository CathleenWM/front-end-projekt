<script setup>
import { ref, onMounted, computed, watch } from 'vue'

const todos = ref([])
const input_content = ref('')

const todos_asc = computed(() => todos.value.sort((a,b) =>{
	  return a.createdAt - b.createdAt
}))

watch(todos, (newVal) => {
 	localStorage.setItem('todos', JSON.stringify(newVal))
 }, {
	deep: true
})

const addTodo = () => {
	if (input_content.value.trim() === '') {
		return
	}
	todos.value.push({
		content: input_content.value,
		done: false,
		editable: false,
		createdAt: new Date().getTime()
	})
}

onMounted(() => {
	todos.value = JSON.parse(localStorage.getItem('todos')) || []
})

</script>

<template>
	<main class="app">

		<section class="create-todo">
			<h3>CREATE A TODO</h3>

			<form id="new-todo-form" @submit.prevent="addTodo">
				<h4>What's on your todo list?</h4>
				<input 
					type="text" 
					placeholder="e.g. make a todo list"
					v-model="input_content" />
				
				<input type="submit" value="Add todo" />
			</form>
		</section>

		<section class="todo-list">
			<h3>Todo list</h3>
			<div class="list" id="todo-list">

				<div v-for="todo in todos_asc" :class="`todo-item ${todo.done && 'done'}`">

					<div class="todo-content">
						<input type="text" v-model="todo.content" />
					</div>
				</div>
			</div>
		</section>

	</main>
</template>