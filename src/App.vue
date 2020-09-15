<template>
  <div>
    <input
      v-model="currentTodo"
      @keydown.enter="addTodo()"
      placeholder="Add a todo"
    />
    <ul class="todos">
      <li v-for="todo in todos" :key="todo.id">
        <div>
          <input
            type="checkbox"
            v-on:click="done(todo)"
            v-bind:checked="todo.done"
          />
          <label @dblclick="todo.edit = true">
            <del v-if="todo.done"> {{ todo.label }} </del>
            <span v-else>{{ todo.label }}</span>
          </label>
          <button v-on:click="removeTodo(todo.id)">X</button>
        </div>
        <input
          v-show="todo.edit == true"
          v-model="todo.label"
          v-on:blur="
            todo.edit = false;
            $emit('update');
          "
          @keyup.enter="
            todo.edit = false;
            $emit('update');
          "
        />
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todos: [],
      currentTodo: "",
      editedTodo: null,
    };
  },
  methods: {
    addTodo() {
      this.todos.push({
        id: this.todos.length,
        label: this.currentTodo,
        completed: false,
        edit: false,
        done: false,
      });
      this.currentTodo = "";
    },
    done(todo) {
      todo.done = !todo.done;
    },
    editTodo(todo) {
      this.editedTodo = todo;
    },
    removeTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
  },
};
</script>

<style></style>
