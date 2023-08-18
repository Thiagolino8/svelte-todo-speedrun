<script lang="ts">
	import { flip } from 'svelte/animate'
	import { fade } from 'svelte/transition'

	interface Todo {
		title: string
		completed: boolean
	}

	const localStorageKey = 'svelte-todos'

	function recoverTodos() {
		return JSON.parse(localStorage.getItem(localStorageKey)) as Todo[] | null
	}

	let todos: Todo[] = recoverTodos() || []
	let newTodoTitle = ''

	function addTodo(title: string) {
		todos.push({ title, completed: false })
		todos = todos
	}

	function removeTodo(title: string) {
		const index = todos.findIndex((todo) => todo.title === title)
		todos.splice(index, 1)
		todos = todos
	}

	function handleSubmit() {
		addTodo(newTodoTitle)
		newTodoTitle = ''
	}

	$: localStorage.setItem(localStorageKey, JSON.stringify(todos))
</script>

<form on:submit|preventDefault={handleSubmit}>
	<input bind:value={newTodoTitle} />
	<button>Add todo</button>
</form>

{#each todos as todo (todo)}
	<div transition:fade animate:flip>
		<input type="checkbox" bind:checked={todo.completed} />
		<input bind:value={todo.title} class:completed={todo.completed} />
		<button on:click={() => removeTodo(todo.title)}>x</button>
	</div>
{/each}

<style>
	.completed {
		text-decoration: line-through;
	}
</style>
