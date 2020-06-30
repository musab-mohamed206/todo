<template>
    <div>
        <input type="text" class="todo-input" placeholder="What needs to be done" v-model="newTodo" @keyup.enter="addTodo">
        <transition-group name="fade" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOutDown">
            <div v-for="(todo , index) in todos" :key="todo.id" class="todo-item">
                <div class="todo-item-left">
                    <input type="checkbox" v-model="todo.completed">
                    <div v-if="!todo.editing" class="todo-item-lable" @dblclick="editTodo(todo)" :class="{ completed : todo.completed }"> {{ todo.title }} </div>
                    <input v-else class="todo-item-edit" type="text" v-model="todo.title" @blur="doneEdit(todo)" @keyup.enter="doneEdit(todo)" v-focus @keyup.esc="cancelEdit(todo)">
                </div>
                <div class="remove-item" @click="removeTodo(index)">
                    &times;
                </div>
            </div>
        </transition-group>
        <div class="extra-container">
            <div><label><input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos">Check All</label></div>
            <div>{{ remaining }} items left</div>
        </div>
    </div>
</template>

<script>
export default {
  name: 'todo-list',
  data () {
    return {
      newTodo: '',
      idForTodo: 3,
      beforeEditCache : '',
      todos: [
          {
              'id' : 1,
              'title' : 'Finish Vue Screencast',
              'completed' : false,
              'editing': false,
          },
          {
              'id' : 2,
              'title' : 'take over world',
              'completed' : false,
              'editing': false,
          }
      ]
    }
  },
  computed: {
      remaining() {
          return this.todos.filter(todo => !todo.completed).length
      },
      anyRemaining () {
          return this.remaining != 0
      },
  },

  directives: {
      focus : {
          inserted: function (el) {
              el.focus()
          }
      }
  },
  methods: {
      addTodo() {
          if (this.newTodo.trim().length == 0) {
              return
          }
          this.todos.push({
              id : this.idForTodo,
              title: this.newTodo,
              completed: false,
              editing: false,
          })
          this.newTodo = ''
          this.idForTodo++
      },

      editTodo(todo) {
          this.beforeEditCache = todo.title
          todo.editing = true
      },
      doneEdit(todo) {
          if (todo.title.trim() == '') {
              todo.title = this.beforeEditCache
          }
          todo.editing = false
      },
      cancelEdit(todo){
          todo.title = this.beforeEditCache
          todo.editing = false
      },

      checkAllTodos() {
          this.todos.forEach((todo) => todo.completed = event.target.checked)
      },

      removeTodo (index) {
          this.todos.splice(index , 1)
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >

@import url(https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css);

 .todo-input {
     width: 100%;
     padding: 10px 18px;
     font-size: 18px;
     margin-bottom: 16px;

 }

 .todo-item {
     margin-bottom: 12px;
     display: flex;
     align-items: center;
     justify-content: space-between;
     animation-duration: 0.3s;
 }

 .remove-item {
     cursor: pointer;
     margin-left: 14px;
 }

 .todo-item-left{
     display: flex;
     align-items: center;
 }
 .todo-item-lable{
     padding: 10px;
     border: 1px solid white;
     margin-left: 12px;
 }
 .todo-item-edit {
     font-size: 24px;
     color: #2c3e50;
     margin-left: 12px;
     width: 100%;
     padding: 10px;
     border: 1px solid #ccc;
     font-family: Arial, Helvetica, sans-serif;
 }

 .completed {
     text-decoration: line-through;
     color: gray;
 }

 .extra-container {
     display: flex;
     align-items: center;
     justify-content: space-between;
     font-size: 16px;
     border-top: 1px solid lightgray;
     padding-top: 14px;
     margin-bottom: 14px;
 }
 button {
     font-size: 14px;
     background-color: white;
     appearance: none;
 }

 .active {
     background: lightgray;
 }
</style>
