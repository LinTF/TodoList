<template>
    <div class="col-md-4">
        <h3># 待辦事項 
            <button class="del-icon" @click="showClass"><i class="fa-regular fa-trash-can"></i></button>
        </h3>
        <div class="todo-block">
            <div v-for="(item, index) in propsTodo" :key="index" class="row item">
                <div class="col-md-8 vertical-center">
                    <input type="checkbox" :id="item+index">
                    <label :for="item+index">{{ item }}</label>
                </div>
                <div class="col-md-4">
                    <button type="submit" class="btn btn-danger mb-3" :style="dynamicStyle" @click="deleteTodoItem(index)">刪除</button>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
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
            showClass() {
                this.isShow = !this.isShow;
            }
        },
        computed: {
            dynamicStyle() {
                return {
                    visibility: this.isShow ? 'unset' : 'hidden',
                }
            }
        }
    }
</script>

<style scoped lang="scss">
    h3 {
        text-align: center;
    }

    .del-icon {
        background-color: unset;
        border: none;
        font-size: 20px;
        color: #555555;
    }

    .todo-block {
        .item {
            background-color: #f6f5f5;
            border-radius: 30px;
            padding: 5px 10px;
            margin: 10px 0px 10px 0px;
            border: 1px solid #f6f5f5;

            &:hover{
                border: 1px solid #d6279c;
            }

            p {
                margin-bottom: 0px;
            }
    
            button {
                margin: 0 auto;
                display: block;
                margin-bottom: 0px !important;
            }

            input[type="checkbox"] {
                margin-right: 10px;
            }
        }
    }

    .vertical-center {
        display: flex;
        align-items: center;
    }
</style>