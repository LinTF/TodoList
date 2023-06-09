<template>
    <div class="col-md-4" v-for="(todo, dateIndex) in propsTodo" :key="dateIndex">
        <h3>{{ todo.date }}
            <button class="del-icon" @click="showClass(todo.date)"><i class="fa-regular fa-trash-can"></i></button>
        </h3>
        <div class="todo-block">
            <div v-for="(todoItem, itemIndex) in todo.item" :key="todoItem.text" class="row item">
                <div class="col-md-8 vertical-center">
                    <input type="checkbox" :id="todoItem.text+itemIndex" 
                        @change="getCheckedItem(dateIndex, itemIndex, todoItem.isFinish)">
                    <label :for="todoItem.text+itemIndex" 
                        :style="{ 'text-decoration': todoItem.isFinish === true ? 'line-through' : 'none' }">
                        {{ todoItem.text }}
                    </label>
                </div>
                <div class="col-md-4">
                    <button type="submit" class="btn btn-danger mb-3" :style="{ visibility: todoItem.showDeleteBtn === true ? 'unset' : 'hidden' }" @click="deleteTodoItem(todo.date, dateIndex, itemIndex)">刪除</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { end } from '@popperjs/core';

    export default {
        name: 'todo',
        props: {
            propsTodo: {
                type: Array,
                required: true
            }
        },
        data() {
            return {
                isShow: false,
                selectItem: -1,
                itemTxtArray: [],
                isChecked: false
            }
        },
        methods: {
            deleteTodoItem(date, dateIndex, itemIndex) {
                // 先找到要刪除的日期
                const dateData = this.propsTodo.find(item => item.date === date);
                // 計算該日期有幾筆資料
                const dataCount = dateData.item.length;

                // 如果資料量大於一筆，則刪除該項目，其他．則將該日期資料全刪除
                if (dataCount > 1) {
                    dateData.item.splice(itemIndex, 1);
                } else {
                    this.propsTodo.splice(dateIndex, 1);
                }

                // 切完之後畫面的刪除按鈕隱藏
                this.showClass(date)
                
                // 將 localStorage 陣列裝回
                localStorage.setItem('todoItem', JSON.stringify(this.propsTodo));
            },
            showClass(dateVal) {
                for (const todo of this.propsTodo) {
                    if (todo.date === dateVal) {
                        for (const item of todo.item) {
                            item.showDeleteBtn = !item.showDeleteBtn;
                        }
        
                        break;
                    }
                }
            },
            getCheckedItem(dateIndex, itemIndex, isFinish) {
                if (isFinish === false) {
                    this.propsTodo[dateIndex].item[itemIndex].isFinish = true;
                }

                if (isFinish === true) {
                    this.propsTodo[dateIndex].item[itemIndex].isFinish = false;
                }
            }
        }
    }
</script>

<style scoped lang="scss">
    .todo-block {
        .item {
            background-color: #f6f5f5;
            border: 1px solid #f6f5f5;

            &:hover{
                border: 1px solid #d6279c;
            }
        }
    }
</style>