<template>
  <div class="container">
    <h1 class="text-center mt-5">Vue 3 Todo</h1>
    <div class="row justify-content-center">
      <div class="col">
        <div class="input-group mb-3">
          <input
            v-model="todo"
            type="text"
            class="form-control"
            placeholder="請輸入代辦事項"
            @keyup.enter="addTodo"
          />
          <button type="button" class="btn btn-primary" @click="addTodo">
            新增
          </button>
        </div>
        
        <ul>
          <li class="row py-1" v-for="(todo, index) in todos" :key="todo.id">
            <div class="col">
              <input
                type="checkbox"
                :value="todo.name"
                v-model="todo.done"
                v-bind:checked="todo.done"
                :id="todo.id"
              />
              <label class="px-2" :for="todo.id" :style="todo.done ? 'text-decoration: line-through' : ''">
                {{ todo.name }}
              </label>
            </div>
            <div class="col-2 d-flex justify-content-end">
              <button type="button" class="btn btn-danger" @click="removeTodo(index)">
                刪除
              </button>
            </div>
          </li>
        </ul>
        <h4 class="text-end">已完成 {{ finishedTodo }} / 未完成 {{ todos.length - finishedTodo }}</h4>       
      </div>
    </div>
  </div>
</template>

<script>
import {ref , reactive, computed} from 'vue'

export default {
  name: "App",
  components: {},
  setup() {
    const todo = ref('');

    let todos = reactive([
      { id: 0, name: "吃飯", done: true },
      { id: 1, name: "睡覺", done: false },
      { id: 2, name: "打東東", done: false },
    ]);

    let finishedTodo = computed({
      get: () => {
        return todos.filter((todo) => todo.done === true).length
      },
      set: () => {}
    })

    function addTodo() {
      if (todo.value.trim() === '') {
        return
      }

      todos.push( {id: Date.now(), name: todo.value, done:false} )

      todo.value = ''
    }

    function removeTodo(index) {
      todos.splice(index , 1)
    }

    return { todo, todos, addTodo, removeTodo, finishedTodo };
  },
};
</script>

<style>
ul {
  list-style-type: none;
  padding-left: 0 !important;
}
</style>
