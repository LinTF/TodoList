<template>
    <div class="col-xl-4 col-md-6 date-black" v-for="(todo, dateIndex) in propsTodo" :key="dateIndex">
        <h3>{{ todo.date }}
            <button class="del-icon" 
                @click="showDelBtn(todo.date)">
                <i class="fa-regular fa-trash-can"></i>
            </button>
        </h3>
        <div class="todo-block">
            <div v-for="(todoItem, itemIndex) in todo.item" :key="todoItem.text" class="row item"
                :style="{ 'background-color': todoItem.isFinish === false ? '#f8e4cc' : '#fff' }">
                <div class="col-7 vertical-center">
                    <input type="checkbox" :id="todo.date+itemIndex"
                        :name="todo.date+itemIndex" 
                        @change="getCheckedItem(dateIndex, itemIndex)"
                        :checked="todoItem.isFinish"
                        :disabled="todoItem.isFinish">
                    <label :for="todo.date+itemIndex" 
                        :style="{ 'text-decoration': todoItem.isFinish === true ? 'line-through' : 'none' }">
                        {{ todoItem.text }}
                    </label>
                </div>
                <div class="col-5 btn-block">
                    <button type="submit" class="btn btn-danger mb-3" 
                    :style="{ visibility: todo.isEdit === true ? 'unset' : 'hidden', 
                              display: todo.isEdit === false && todoItem.isFinish === true ? 'none' : 'block' }" 
                    @click="deleteTodoItem(todo.date, dateIndex, itemIndex, todoItem.text)">刪除</button>

                    <button type="submit" class="btn return mb-3" 
                    :style="{ display: todo.isEdit === false && todoItem.isFinish === true ? 'block' : 'none' }" 
                    @click="getCheckedItem(dateIndex, itemIndex)">取消勾選</button>
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
            }
        },
        methods: {
            deleteTodoItem(date, dateIndex, itemIndex, itemText) {
                const checkDelete = confirm("您確定要刪除 " + date + '-' + itemText + " 嗎？");
                if (checkDelete) {
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
                    this.showDelBtn(date)
                    
                    // 將 localStorage 陣列裝回
                    localStorage.setItem('todoItem', JSON.stringify(this.propsTodo));
                }
            },
            showDelBtn(dateVal) {
                for (const todo of this.propsTodo) {
                    if (todo.date === dateVal) {
                        todo.isEdit = !todo.isEdit;
                    } else {
                        todo.isEdit = false;
                    }
                }
            },
            getCheckedItem(dateIndex, itemIndex) {
                console.log(dateIndex + ',,' + itemIndex)

                this.propsTodo[dateIndex].item[itemIndex].isFinish = !this.propsTodo[dateIndex].item[itemIndex].isFinish

                // 將 localStorage 陣列裝回
                localStorage.setItem('todoItem', JSON.stringify(this.propsTodo));
            }
        }
    }
</script>

<style scoped lang="scss">
    .todo-block {
        padding: 0px 10px 10px 10px;
        border-radius: 10px;
        background-color: #fff;

        .item {
            border: 1px solid #f8e4cc;

            &:hover{
                border: 1px solid #d6279c;
            }
        }

        .return {
            color: #d4ac7c;
            font-size: 14px;
        }

        .btn-danger {
            font-size: 14px;
        }
    }
    
    .date-black {
        margin-bottom: 30px;
    }
</style>