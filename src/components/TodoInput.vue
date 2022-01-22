<template>
  <div class="inputBox shadow">
      <!-- 
        v-model이라는 속성을 사용하면 입력 값이 자동으로 뷰 데이터 속성에 연결됩니다.
        v-moel = newTodoItem >>
        data > newTodoItem: "inputed data"
      -->
      <input type="text" v-model="newTodoItem" placeholder="Type what you have to do" v-on:keyup.enter="addTodo">
      <span class="addContainer" v-on:click="addTodo">
          <i class="addBtn fas fa-plus" aria-hidden="true"></i>
      </span>

      <Modal v-if="showModal" @close="showModal = false">
          <template v-slot:header>
              <div>
                  <h3>경고</h3>
              </div>
          </template>
          <template v-slot:body>
              <div>
                  할 일을 입력하세요
              </div>
          </template>
          <template v-slot:footer>
              <div>
              <i class="closeModalBtn fas fa-times" aria-hidden="true"  @click="showModal = false"></i>    
              </div>
          </template>
      </Modal>
  </div>
</template>

<script>
import Modal from './common/Modal.vue';
export default {
    data() {
        return {
            newTodoItem: '',
            showModal: false
        }
    },
    components: {
        Modal : Modal
    },
    methods: {
        addTodo() {
            if(this.newTodoItem !== ""){
                let value = this.newTodoItem && this.newTodoItem.trim();
                this.$emit('addTodo', value);
                this.clearInput();
            }

            else {
                this.showModal = !this.showModal;
            }
            
        },
        clearInput(){
            this.newTodoItem = "";
        }
    }
}
</script>

<style scoped>
    input:focus {
        outline: none;
    }
    .inputBox {
        background: white;
        height: 50px;
        line-height: 50px;
        border-radius: 5px;
    }
    .inputBox input {
        border-style: none;
        font-size: 0.9rem;
    }
    .addContainer {
        float: right;
        background: linear-gradient(to right, #6478fb, #8763fb);
        display: block;
        width: 3rem;
        border-radius: 0 5px 5px 0;
    }
    .addBtn {
        color: white;
        vertical-align: middle;
    }
</style>