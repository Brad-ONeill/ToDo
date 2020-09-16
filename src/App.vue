<template>
  <div id="container">
    <div class="md-layout">
      <div class="md-layout-item">
        <md-card>
          <md-card-content>
            <md-card-header>Your <b>To Do</b> list</md-card-header>
            <div>
              <md-card-content>
                <input
                  class="todoInput"
                  v-model="currentTodo"
                  @keydown.enter="addTodo()"
                  placeholder="Add a todo"
                />

                <ul class="todos">
                  <li v-for="todo in todos" :key="todo.id">
                    <div>
                      <span>
                        <input
                          class="md-primary"
                          type="checkbox"
                          v-on:click="done(todo)"
                          v-bind:checked="todo.done"
                        />
                      </span>

                      <span id="todoItem">
                        <label @dblclick="todo.edit = true">
                          <del v-if="todo.done"> {{ todo.label }} </del>
                          <span v-else>{{ todo.label }}</span>
                        </label>
                      </span>

                      <span class="xButton">
                        <md-button
                          class="md-accent"
                          v-on:click="removeTodo(todo.id)"
                          >&times;</md-button
                        >
                      </span>
                    </div>

                    <input
                      id="todoEdit"
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
              </md-card-content>
            </div>
          </md-card-content>
        </md-card>
      </div>
    </div>
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

<style>
body {
  display: flex;
  justify-content: center;
  align-items: top;
  padding: 50px;
  background-color: lightgray;
}

#container {
  display: flex;
  justify-content: center;
  width: 100%;
}

.md-layout {
  min-width: 60%;
}

.md-card-header {
  font-size: 22px;
  text-align: center;
}

#todoEdit {
  font-family: "Courier New", Courier, monospace;
  margin: 0 0 5px 40px;
  border: 1px dotted gray;
  border-radius: 4px;
  padding: 4px;
  color: darkred;
  font-size: 12px;
}

textarea:focus,
input:focus {
  outline: none;
}

.todoInput {
  font-family: Arial, Helvetica, sans-serif;
  width: 100%;
  border-top: 0;
  border-right: 0;
  border-bottom: 1px dotted;
  border-left: 0;
  padding: 0 0 5px 0;
  margin-bottom: 20px;
}

ul {
  padding: 0;
  margin: 0;
}

li {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

#todoItem {
  padding: 0 15px;
  vertical-align: top;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

button.md-button {
  align-items: flex-start;
  margin: 0 5px;
  min-width: 20px;
  width: 20px;
  height: 20px;
}

.md-button {
  min-width: 20px;
  margin: 0;
  padding: 0;
}

.xButton {
  float: right;
}
</style>
