<template>
  <div id="edit-block">
    <div class="row justify-content-md-center">
      <div class="col-auto">
        <label for="todoItem" class=" col-form-label">待辦事項：</label>
      </div>
      <div class="col-md-6">
        <input type="text" class="form-control" id="todoItem" placeholder="請輸入待辦事項" v-model="todoItemText">
      </div>
      <div class="col-auto">
        <button type="submit" class="btn btn-primary mb-3" @click="addTodoItem">新增</button>
      </div>
    </div>
  </div>

  <div class="row">
    <todo :propsTodo="todoItem" @emitItemTxtArray="getSelectItemTxt" />
    <ing :propsIngItemTxtArray="ingItem" />
  </div>
</template>

<script>
  import todo from "@/components/todo.vue"
  import ing from "@/components/ing.vue"

  export default {
    name: 'todoList',
    data() {
      return {
        todoItem: [],
        ingItem: [],
        finishItem: [],
        todoItemText: ''
      }
    },
    components: {
      todo,
      ing
    },
    created() {
      // 當元件被建立時讀取 localstorage 資料
      const todoListData = localStorage.getItem("todoItem");
      if (todoListData) {
        this.todoItem = JSON.parse(todoListData);
      }
    },
    methods: {
      addTodoItem() {
        if (this.todoItemText.trim() !== '') {
          this.todoItem.push(this.todoItemText);
          localStorage.setItem("todoItem", JSON.stringify(this.todoItem));

          // 新增後還原 textbox 為空值
          this.todoItemText='';
        }
      },
      getSelectItemTxt(val) {
        this.ingItem = val;
      }
    }
  }
</script>

<style scoped lang="scss">
  #edit-block {
    margin-top: 20px;
  }
</style>