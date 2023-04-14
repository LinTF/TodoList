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
  <div>
    <p v-for="(item, index) in todoItem" :key="index">{{ item }}</p>
  </div>
</template>

<script>
  export default {
    name: 'todo',
    data() {
      return {
        todoItem: [],
        ingItem: [],
        finishItem: [],
        todoItemText: ''
      }
    },
    methods: {
      addTodoItem() {
        this.todoItem.push(this.todoItemText);
        
        // 新增後還原 textbox 為空值
        this.todoItemText='';
      }
    }
  }
</script>

<style scoped lang="scss">
  #edit-block {
    margin-top: 20px;
  }
</style>