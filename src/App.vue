<template>
  <div id="edit-block">
    <div class="row justify-content-md-center">
      <div class="col-auto">
        <label for="todoItem" class=" col-form-label">待辦事項：</label>
      </div>
      <div class="col-md-2">
        <div class="dropdown">
          <div>
            <input type="date" v-model="selectedDate" class="form-control">
          </div>
        </div>
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
    <todo :propsTodo="todoItem" />
    <!-- <ing :propsIngItemTxtArray="ingItem" /> -->
  </div>
</template>

<script>
  import todo from "@/components/todo.vue"
  import ing from "@/components/ing.vue"

  export default {
    name: 'todoList',
    data() {
      return {
        todoItem: [
          {
            date: '2023/05/19',
            item: [
              { text: '這是第一個測試', isFinish: false, showDeleteBtn: false},
              { text: '這是第二個測試', isFinish: false, showDeleteBtn: false },
              { text: '這是第三個測試', isFinish: false, showDeleteBtn: false }
            ]
          },
          {
            date: '2023/05/20',
            item: [
              { text: '這是第一個測試1', isFinish: false, showDeleteBtn: false },
              { text: '這是第二個測試2', isFinish: false, showDeleteBtn: false }
            ]
          }
        ],
        todoItemText: '',
        selectedDate: ''
      }
    },
    components: {
      todo,
      ing
    },
    created() {
      // 當元件被建立時讀取 localstorage 資料
      // const todoListData = localStorage.getItem("todoItem");
      // if (todoListData) {
      //   this.todoItem = JSON.parse(todoListData);
      // }

      // 日期元件
      const today = new Date();
      const year = today.getFullYear();
      const month = String(today.getMonth() + 1).padStart(2, '0');
      const day = String(today.getDate()).padStart(2, '0');
      this.selectedDate = `${year}-${month}-${day}`;
    },
    methods: {
      addTodoItem() {
        const todoItemText = this.todoItemText.trim();
        if (todoItemText !== '') {
          const formatSelDate = this.formatDate(this.selectedDate);
          const hasDateData = this.todoItem.find(item => item.date === formatSelDate);

          // *** todoItem 陣列結構 [
          //   {
          //     date: '2023/05/19',
          //     item: [
          //       { text: '待辦事項', isFinish: false }
          //     ]
          //   }
          // ]
          
          console.log(hasDateData)
          if (hasDateData) {
            hasDateData.item.push({ text: todoItemText, isFinish: false });
          } else {
            
          }


          // 將更新後的 todoItem 陣列存入 localStorage
          localStorage.setItem("todoItem", JSON.stringify(this.todoItem));

          // 新增後還原 textbox 為空值
          // this.todoItemText='';
        }
      },
      formatDate(dateStr) {
        const date = new Date(dateStr);
        const year = date.getFullYear();
        const month = String(date.getMonth() + 1).padStart(2, '0');
        const day = String(date.getDate()).padStart(2, '0');

        // 格式化日期
        return `${year}/${month}/${day}`;
      }
    }
  }
</script>

<style scoped lang="scss">
  #edit-block {
    margin-top: 20px;
  }

  .dropdown {
    text-align: center;
  }
</style>