<template>
  <div id="edit-block">
    <div class="row justify-content-md-center">
      <div class="col-auto">
        <label for="todoItem" class=" col-form-label">待辦事項：</label>
      </div>
      <div class="col-md-2">
        <div class="dropdown">
          <button class="btn btn-secondary dropdown-toggle" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
            {{ formatDate(this.dates[0]) }}
          </button>
          <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
            <li v-for="date in this.dates" :key="date"><a class="dropdown-item" href="#">{{ formatDate(date) }}</a></li>
          </ul>
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
        todoItemText: '',
        dates: []
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

      // 日期列表
      this.getDateList();
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
      },
      getDateList() {
        const now = new Date();
        const after30Days = new Date(now);
        // const dateList = [];

        // 加 30 天
        after30Days.setDate(now.getDate() + 30);

        // 跑迴圈把所有日期裝到陣列 dates 裡
        while (now <= after30Days) {
          // dateList.push(new Date(now));
          this.dates.push(new Date(now));
          now.setDate(now.getDate() + 1);
        }

        // this.dates = dateList;
      },
      formatDate(date) {
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