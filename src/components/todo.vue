<template>
    <div class="col-md-4">
        <h3># 待辦事項 
            <button class="del-icon" @click="showClass"><i class="fa-regular fa-trash-can"></i></button>
        </h3>
        <div class="todo-block">
            <div v-for="(item, index) in propsTodo" :key="index" class="row item">
                <div class="col-md-8 vertical-center">
                    <input type="checkbox" :id="item+index" :checked="selectItem === index" @change="getCheckedItem(index, item)">
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
                selectItem: -1,
                itemTxtArray: []
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
            },
            getCheckedItem(val, txt) {
                this.selectItem = val;
                if (this.selectItem !== -1) {
                    this.propsTodo.splice(val, 1);
                    this.itemTxtArray.push(txt);
                    this.$emit('emitItemTxtArray', this.itemTxtArray);
                }
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