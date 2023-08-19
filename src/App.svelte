<script lang="ts">
	import { flip } from 'svelte/animate'
	import { fade } from 'svelte/transition'

	interface Todo {
		title: string
		completed: boolean
	}

	const storageKey = 'svelte-todos'

	let todos: Todo[] = JSON.parse(localStorage.getItem(storageKey)) || []
	let newTodoTitle = ''

	function handleSubmit() {
		todos = [...todos, { title: newTodoTitle, completed: false }]
		newTodoTitle = ''
	}

	function deleteTodo(title: string) {
		todos = todos.filter((todo) => todo.title !== title)
	}

	$: localStorage.setItem(storageKey, JSON.stringify(todos))
</script>

<form on:submit|preventDefault={handleSubmit}>
	<input bind:value={newTodoTitle} />
	<button>Add Todo</button>
</form>

{#each todos as todo (todo)}
	<div animate:flip transition:fade>
		<input type="checkbox" bind:checked={todo.completed} />
		<input bind:value={todo.title} class:completed={todo.completed} />
		<button on:click={() => deleteTodo(todo.title)}>x</button>
	</div>
{/each}

<style>
	.completed {
		text-decoration: line-through;
	}
</style>
