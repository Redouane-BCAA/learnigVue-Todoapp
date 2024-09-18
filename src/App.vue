<template>
  <form action="" @submit.prevent="addTodo">
    <fieldset role="group">
      <input 
      v-model="newTodo"
      type="text" 
      placeholder="Tâche à éffectuer">
      <button :disabled="newTodo.length == 0">Ajouter</button>
    </fieldset>
    <div v-if="todos.length == 0"> Vous n'avez pas de tâches à faire</div>
    <div v-else>
      <ul>
        <li 
        v-for="todo in sortedTodo()"
        :key="todo.date"
        :class="{completed : todo.completed}"
        >
        <label>
          <input type="checkbox" v-model="todo.completed">
          {{ todo.title }}
        </label>
      </li>
      </ul>
      <label>
        <input type="checkbox" v-model="hideCompleted">
        Masquer les tâches complétées
      </label>
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";

const todos = ref([
	{
		title: "faire les courses",
		completed: true,
		date: 1,
	},
	{
		title: "faire à manger",
		completed: false,
		date: 2,
	},
]);
const newTodo = ref("");
const hideCompleted = ref(false)
const addTodo = () => {
	todos.value.push({
		title: newTodo.value,
		completed: false,
		date: Date.now(),
	});
	newTodo.value = "";
};

const sortedTodo = () => {
  const sortedTodos=  todos.value.toSorted((a, b) => a.completed > b.completed ? 1 : -1)

  if(hideCompleted.value === true){
    return sortedTodos.filter(t => t.completed === false)
  }
  return sortedTodos
}
</script>

<style>
.completed{
  opacity: .5;
  text-decoration:line-through;
}
</style>