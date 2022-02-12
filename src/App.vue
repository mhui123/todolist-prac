<template>
<div id="app">
  <TodoHeader></TodoHeader>
  <TodoInput v-on:addTodo="addTodo"></TodoInput>
  <TodoList 
    @changeCheck = "showCheck = !showCheck; checked = $event;" 
    @removeTodo="removeTodo"
    @toFixItem="toFix = !toFix;"
    @fixContent="fixContent($event); showCheck = !showCheck;"
    :propsdata="todoItems" :showCheck="showCheck" :checked="checked" :toFix="toFix"></TodoList>
  <TodoFooter v-on:removeAll="clearAll"></TodoFooter>
</div>
</template>

<script>
import TodoHeader from './components/TodoHeader.vue';
import TodoFooter from './components/TodoFooter.vue';
import TodoInput from './components/TodoInput.vue';
import TodoList from './components/TodoList.vue';

export default {
  name: 'App',
  data() {
    return {
      todoItems:[],
      showCheck:false,
      checked: 0,
      toFix: false,

    }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoFooter': TodoFooter,
    'TodoInput': TodoInput,
    'TodoList': TodoList
  },
  created(){
    if(localStorage.length > 0){
        for (let i = 0; i < localStorage.length; i ++){
            let sKey = localStorage.key(i);
            let target = localStorage.getItem(sKey);
            if(target !== "SILENT" && target !== "undefined"){
              this.todoItems.push(localStorage.key(i));
            }
        }
    }
  },

  methods: {
    addTodo(todoItem){
      localStorage.setItem(todoItem, todoItem); // setItem(key, value);
      this.todoItems.push(todoItem);
    },
    clearAll() {
      localStorage.clear();
      this.todoItems = [];
    },
    removeTodo(todoItem, index){
      localStorage.removeItem(todoItem);
      this.todoItems.splice(index, 1); // splice(startIdx, count): 배열에서 인덱스 시작지점부터 count만큼 삭제
    },
    fixContent(todoItem){
      let lastIdx = this.todoItems.length -1;
      
      this.tempArr = this.todoItems.splice(this.checked, (this.todoItems.length - this.checked), todoItem); //수정할 타겟부터 끝까지 잘라내고, todoItems의 잘라낸 자리에 새로운 값 넣음 
      //this.todoItems.push(todoItem); //수정한 데이터 삽입

      if(this.checked !== lastIdx){ //체크한게 배열 마지막 값이 아니면
        let tempArr2 = this.tempArr.splice(1, this.tempArr.length -1); //최초값은 수정할 데이터이므로 제거
        this.tempArr = this.todoItems.concat(tempArr2);
        this.todoItems = this.tempArr;
      }

      //로컬스토리지 수정
      localStorage.clear();//기존 로컬스토리지 삭제
      for (let i = 0; i < this.todoItems.length; i++) { //새 데이터 삽입
        localStorage.setItem(this.todoItems[i], this.todoItems[i]);
      }
      
    }
  }
}
</script>

<style>
body {
  text-align: center;
  background-color: #f6f6f8
}

input {
  border-style: groove;
  width: 200px;
}

button {
  border-style: groove;
}

.shadow {
  box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03);
}
</style>
