<template>
	<v-app>
		<v-main>
			<Header />
			<AddTodo v-on:addTodo="addTodo" />
			<div class="legend">
				<span>
					<span>Double click to mark as complete</span>
				</span>
				<span> <span class="completeBox"></span>=Complete </span>
				<span> <span class="incompleteBox"></span>=Incomplete </span>
			</div>

			<Todos
				class="todos"
				v-bind:todos="todos"
				v-on:del-todo="deleteTodo"
				v-on:completed="completedChanged"
			/>
		</v-main>
	</v-app>
</template>

<script>
import Todos from "./components/Todos";
import Header from "./components/layout/header";
import AddTodo from "./components/AddTodo";
import Firebase from "firebase";
var database = Firebase.initializeApp({
	apiKey: "AIzaSyA4dTMznV_LRSJqiD02hmxsAaVCWpVtzDU",
	authDomain: "vue-todo-147ad.firebaseapp.com",
	databaseURL: "https://vue-todo-147ad.firebaseio.com",
	projectId: "vue-todo-147ad",
	storageBucket: "vue-todo-147ad.appspot.com",
	messagingSenderId: "997636696575",
	appId: "1:997636696575:web:fd297f6c8846b6b9825fca",
	measurementId: "G-KGLJL9JTM8",
})
	.database()
	.ref();
//import firebase again, set up crud, good to go

export default {
	name: "App",

	components: { Todos, Header, AddTodo },

	data() {
		return {
			todos: [],
		};
	},
	methods: {
		deleteTodo(id) {
			let index = this.todos.findIndex((x) => x.id === id);

			var ref = this.todos[index].ref;

			ref && ref.remove();

			this.todos = this.todos.filter((todo) => todo.id !== id);
		},
		addTodo(newTodo) {
			newTodo.ref = database.push(newTodo);
			this.todos.push(newTodo);
		},
		completedChanged(todo) {
			todo.ref.update({
				completed: todo.completed,
			});
		},
	},
	created() {
		database.once("value", (t) => {
			t.forEach((todo) => {
				this.todos.push({
					ref: todo.ref,
					title: todo.child("title").val(),
					id: todo.child("id").val(),
					completed: todo.child("completed").val(),
				});
			});
		});
	},
};
</script>
<style scoped>
.todos {
	display: grid;
	grid-template-columns: repeat(3, 1fr);
	gap: 1rem;
}
.legend {
	display: flex;
	justify-content: space-around;
	margin-bottom: 1rem;
}

.incompleteBox {
	display: inline-block;
	width: 10px;
	height: 10px;
	background: #00ff95;
}
.completeBox {
	display: inline-block;
	width: 10px;
	height: 10px;
	background: rgb(245, 73, 73);
}
</style>
