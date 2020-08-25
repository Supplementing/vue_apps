<template>
	<div>
		<form @submit="addTodo">
			<!-- v-model is a two way bind so you can update the vlue of title as you type -->
			<input
				type="text"
				v-model="title"
				name="title"
				placeholder="add todo.."
			/>
			<v-menu
				ref="menu1"
				v-model="menu1"
				:close-on-content-click="false"
				transition="scale-transition"
				offset-y
				max-width="290px"
				min-width="290px"
			>
				<template v-slot:activator="{ on, attrs }">
					<v-text-field
						v-model="dateFormatted"
						label="Date"
						hint="MM/DD/YYYY format"
						persistent-hint
						v-bind="attrs"
						@blur="date = parseDate(dateFormatted)"
						v-on="on"
					></v-text-field>
				</template>
				<v-date-picker
					v-model="date"
					no-title
					@input="menu1 = false"
				></v-date-picker>
			</v-menu>

			<input type="submit" value="Submit" class="btn" />
		</form>
	</div>
</template>
<script>
import uuid from "uuid";
export default {
	name: "AddTodo",

	data() {
		return {
			title: "",
		};
	},
	methods: {
		addTodo(e) {
			e.preventDefault();
			const newTodo = {
				id: uuid.v4(),
				title: this.title,
				completed: false,
				date: this.date,
			};
			//send to parent
			this.$emit("addTodo", newTodo);
			this.title = "";
			this.date = "";
		},
	},
};
</script>
<style scoped>
form {
	display: flex;
	background: #d6d6d6;
}

input[type="text"] {
	flex: 10;
	padding: 5px;
}

input[type="submit"] {
	flex: 1;

	background: #00ff95;

	border-radius: 5px;
}
.btn:hover {
	background: #03c574;
}
</style>
