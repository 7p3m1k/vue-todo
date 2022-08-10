<template>
  <h1>TODO-LIST</h1>
  <div>할일 : {{todoList.length}}</div>
  <div>
    <div>
      <input type="text" placeholder="할 일 추가" v-model="todo" @keyup.enter="addTodo"/>
      <button @click="addTodo">추가</button>
    </div>
    <ul>
      <li v-for="(todo,index) in todoList" :key="index">
        <input type="checkbox" v-model="todo.done" />
        <span v-bind:class="{active : todo.done}">{{todo.text}}</span>
        <button @click="remove(todo,index)">삭제</button>
      </li>
    </ul>
    <button @click="removeCheck">선택 삭제</button>
  </div>
</template>

<script>
import { ref,reactive,onMounted,watchEffect} from 'vue'
export default {
  name: 'HelloWorld',

  setup(effect, options){
    let todo = ref('');
    let todoList = reactive([]);

    const addTodo = () => {
      if(todo.value !== ''){
        todoList.push({
          done:false,
          text: todo.value,
        })
        console.log(todo)
        todo.value = '';
      }
      console.log(todoList, 'todolist');
    }

    const remove = (e,i) => {
      if(e.done){
        todoList.splice(i,1)
      }else {
        alert('삭제를 원하시면 체크 해주세요.')
      }
    }

    const removeCheck=()=>{
      todoList = todoList.filter(e => {return e.done !== true})
      console.log(todoList)
    }

    onMounted(()=> {
      if(localStorage.getItem('todoList')){

        this.todoList = JSON.parse(localStorage.getItem('todoList'))
      }else{
        this.todoList = []
      }
    })

    watchEffect(todoList, (newData, prevData) => {
      console.log('감지', newData, prevData)
      console.log('newAge', newData, '에프터todoList', prevData)
      //
      localStorage.setItem('todoList', JSON.stringify(prevData));

    }, {immediate: true})

    return {todo,addTodo,todoList,remove,removeCheck}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1 {
  color: blue;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
.active {
  color: red;
  text-decoration: line-through;
}
</style>
