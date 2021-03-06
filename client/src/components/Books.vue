<template>
  <div class="container">
    <div class="row">
      <div class="col-sm-10">
        <h1>Todos</h1>
        <hr><br><br>
        <alert :message=message v-if="showMessage"></alert>
        <button type="button" class="btn btn-success btn-sm" v-b-modal.todo-modal>Add Todo</button>
        <br><br>

        <!-- todos table -->
        <table class="table table-hover">
          <thead>
            <tr>
              <th scope="col">Todo</th>
              <th scope="col">Author</th>
              <th scope="col">Done?</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(todo, index) in todos" :key="index">
              <td>{{ todo.todo }}</td>
              <td>{{ todo.assignee }}</td>
              <td>
                <span v-if="todo.done">Yes</span>
                <span v-else>No</span>
              </td>
              <td>
                <button type="button"
                        class="btn btn-warning btn-sm"
                        v-b-modal.todo-update-modal
                        @click="editTodo(todo)">
                    Update
                </button>
                <button type="button"
                        class="btn btn-danger btn-sm"
                        @click="onDeleteTodo(todo)">
                    Delete
                </button>
              </td>
            </tr>
          </tbody>
        </table>

      </div>
    </div>

    <!-- add todo modal -->
    <b-modal ref="addTodoModal"
             id="todo-modal"
            todo="Add a new todo"
            hide-footer>
      <b-form @submit="onSubmit" @reset="onReset" class="w-100">
        <b-form-group id="form-todo-group"
                      label="Todo:"
                      label-for="form-todo-input">
            <b-form-input id="form-todo-input"
                          type="text"
                          v-model="addTodoForm.todo"
                          required
                          placeholder="Enter todo">
            </b-form-input>
        </b-form-group>
        <b-form-group id="form-assignee-group"
                      label="Author:"
                      label-for="form-assignee-input">
          <b-form-input id="form-assignee-input"
                        type="text"
                        v-model="addTodoForm.assignee"
                        required
                        placeholder="Enter assignee">
          </b-form-input>
        </b-form-group>

        </b-form-group>
        <b-form-group id="form-done-group">
            <b-form-checkbox-group v-model="addTodoForm.done" id="form-checks">
              <b-form-checkbox value="true">Done?</b-form-checkbox>
            </b-form-checkbox-group>
        </b-form-group>
        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </b-modal>

    <b-modal ref="editTodoModal"
             id="todo-update-modal"
             todo="Update"
             hide-footer>
      <b-form @submit="onSubmitUpdate" @reset="onResetUpdate" class="w-100">
        <b-form-group id="form-todo-edit-group"
                      label="Todo:"
                      label-for="form-todo-edit-input">
          <b-form-input id="form-todo-edit-input"
                        type="text"
                        v-model="editForm.todo"
                        required
                        placeholder="Enter todo">
          </b-form-input>
        </b-form-group>
        <b-form-group id="form-assignee-edit-group"
                      label="Author:"
                      label-for="form-assignee-edit-input">
          <b-form-input id="form-assignee-edit-input"
                        type="text"
                        v-model="editForm.assignee"
                        required
                        placeholder="Enter assignee">
          </b-form-input>
        </b-form-group>
        <b-form-group id="form-done-edit-group">
          <b-form-checkbox-group v-model="editForm.done" id="form-checks">
            <b-form-checkbox value="true">Done?</b-form-checkbox>
          </b-form-checkbox-group>
        </b-form-group>
        <b-button type="submit" variant="primary">Update</b-button>
        <b-button type="reset" variant="danger">Cancel</b-button>
      </b-form>
    </b-modal>
  </div>
</template>

<script>
import axios from 'axios';
import Alert from './Alert';

export default {
  data() {
    return {
      todos: [],
      addTodoForm: {
        todo: '',
        assignee: '',
        done: []
      },
      editForm: {
        id: '',
        todo: '',
        assignee: '',
        done: []
      },
      message: '',
      showMessage: false,
    };
  },
  components: {
    alert: Alert,
  },
  methods: {
    getTodos() {
      const path = 'http://localhost:8080/todos';
      axios.get(path)
        .then((res) => {
          this.todos = res.data.todos;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
        });
    },
    addTodo(payload) {
      const path = 'http://localhost:8080/todos';
      axios.post(path, payload)
        .then(() => {
          this.getTodos();
          this.message = 'Todo added!';
          this.showMessage = true;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
          this.getTodos();
        });
    },
    updateTodo(payload, todoID) {
      const path = `http://localhost:8080/todos/${todoID}`;
      axios.put(path, payload)
        .then(() => {
          this.getTodos();
          this.message = 'Todo updated!';
          this.showMessage = true;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
          this.getTodos();
        });
    },
    removeTodo(todoID) {
      const path = `http://localhost:8080/todos/${todoID}`;
      axios.delete(path)
        .then(() => {
          this.getTodos();
          this.message = 'Todo removed!';
          this.showMessage = true;
        })
        .catch((error) => {
          // eslint-disable-next-line
          console.error(error);
          this.getTodos();
        });
    },
    initForm() {
      this.addTodoForm.todo = '';
      this.addTodoForm.assignee = '';
      this.addTodoForm.done = [];
      this.editForm.id = '';
      this.editForm.todo = '';
      this.editForm.assignee = '';
      this.editForm.done = [];
      this.editForm.id = '';
    },
    onSubmit(evt) {
      evt.preventDefault();
      this.$refs.addTodoModal.hide();
      let done = false;
      if (this.addTodoForm.done[0]) done = true;
      const payload = {
        todo: this.addTodoForm.todo,
        assignee: this.addTodoForm.assignee,
        done // property shorthand
      };
      this.addTodo(payload);
      this.initForm();
    },
    onSubmitUpdate(evt) {
      evt.preventDefault();
      this.$refs.editTodoModal.hide();
      let done = false;
      if (this.editForm.done[0]) done = true;
      const payload = {
        todo: this.editForm.todo,
        assignee: this.editForm.assignee,
        done // property shorthand
      };
      this.updateTodo(payload, this.editForm.id);
    },
    onReset(evt) {
      evt.preventDefault();
      this.$refs.addTodoModal.hide();
      this.initForm();
    },
    onResetUpdate(evt) {
      evt.preventDefault();
      this.$refs.editTodoModal.hide();
      this.initForm();
      this.getTodos(); // why?
    },
    onDeleteTodo(todo) {
      this.removeTodo(todo.id);
    },
    editTodo(todo) {
      this.editForm = todo;
    },
  },
  created() {
    this.getTodos();
  },
};
</script>
