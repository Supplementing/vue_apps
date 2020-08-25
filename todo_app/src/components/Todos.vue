<template>
	<div>
		<div v-bind:key="todo.id" v-for="todo in todos">
			<!--
				the v-on del-todo is used to emit a signal that something shpuld be happening and sends a payload with it to be caught in the next layer
				-->

			<TodoItem
				v-bind:todo="todo"
				v-on:del-todo="$emit('del-todo', todo.id)"
				v-on:completed="$emit('completed', todo)"
			/>
		</div>
	</div>
</template>

<script>
import TodoItem from "./TodoItem.vue";
export default {
	name: "Todos",
	props: ["todos"],
	components: {
		TodoItem,
	},
};
</script>

<style scoped></style>
