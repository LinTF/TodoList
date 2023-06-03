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
                    <button type="submit" class="btn btn-danger mb-3" :style="{ visibility: todoItem.showDeleteBtn === true ? 'unset' : 'hidden' }" @click="deleteTodoItem(itemIndex)">刪除</button>
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
            deleteTodoItem(val) {
                // 從 localStorage 將 todoItem 取出，並轉換成陣列
                const listItem = JSON.parse(localStorage.getItem('todoItem'));
                // val = index 如果 val >= 0，執行陣列的刪除，(val, 1) = 第 index 個，長度 1
                if (val >= 0) {
                    // localStorage 陣列刪除
                    listItem.splice(val, 1);
                    // 顯示的陣列刪除
                    this.propsTodo.splice(val, 1);
                }
                // 將 localStorage 陣列裝回
                localStorage.setItem('todoItem', JSON.stringify(listItem));
            },
            showClass(dateVal) {
                // console.log(this.propsTodo[dateVal])

                for (const todo of this.propsTodo) {
                    // console.log(todo.date)
                    // console.log(dateVal)
                    if (todo.date === dateVal) {
                        // this.isShow = !this.isShow;

                        for (const item of todo.item) {
                            item.showDeleteBtn = !item.showDeleteBtn;
                        }
                        

                        break;
                    }
                }

                // if (this.propsTodo[dateVal] === dateVal) {
                //     console.log('ok')
                //     this.isShow = !this.isShow;
                // }  
            },
            getCheckedItem(dateIndex, itemIndex, isFinish) {
                if (isFinish === false) {
                    this.propsTodo[dateIndex].item[itemIndex].isFinish = true;
                }

                if (isFinish === true) {
                    this.propsTodo[dateIndex].item[itemIndex].isFinish = false;
                }
            }
        },
        computed: {
            dynamicStyle() {
                return {
                    // visibility: this.isShow ? 'unset' : 'hidden'
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