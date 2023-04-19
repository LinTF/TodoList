<template>
    <div class="col-md-4">
        <h3># 正在進行中 
            <button class="del-icon" @click="showClass"><i class="fa-regular fa-trash-can"></i></button>
        </h3>
        <div class="todo-block">
            <div v-for="(item, index) in propsIng" :key="index" class="row item">
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
        name: 'ing',
        props: {
            propsIng: {
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
            showClass() {
                this.isShow = !this.isShow
            }
        },
        computed: {
            dynamicStyle() {
                return {
                    visibility: this.isShow ? 'unset' : 'hidden'
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