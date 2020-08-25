<template>
	<!-- 
    The vbind below is binding the styling of the is complete class to only shpw up based on the todo.completed value
    -->
	<div class="todo-item" v-bind:class="{ 'is-complete': todo.completed }">
		<p v-on:dblclick="markComplete">
			Title: {{ todo.title }}
			<br />
			Date: {{ todo.date }}
			<button @click="$emit('del-todo', todo.id)" class="del">x</button>
		</p>
	</div>
</template>

<script>
export default {
	name: "TodoItem",
	props: ["todo"],
	methods: {
		markComplete() {
			this.todo.completed = !this.todo.completed;
			this.$emit("completed", this.todo);
		},
	},
};
</script>

<style scoped>
.todo-item {
	background: #00ff95;
	padding: 1rem;
	border-bottom: 1px #ccc dotted;
	border-radius: 5px;
	text-align: center;
	position: relative;
}

.is-complete {
	text-decoration: line-through;
	background: rgb(245, 73, 73);
}

.del {
	background: #ff0000;
	color: #fff;
	border: none;
	padding: 5px 9px;
	border-radius: 50%;
	cursor: pointer;
	float: right;
}
</style>
