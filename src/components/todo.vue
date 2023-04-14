<template>
    <div class="col-md-4">
        <h3>待辦事項</h3>
        <div class="todo-block">
            <p v-for="(item, index) in this.propsTodo" :key="index">{{ item }}</p>
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
        }
    }
</script>

<style scoped lang="scss">
    .todo-block {
        background-color: #ccc;
    }
</style>