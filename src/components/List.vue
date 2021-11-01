<template>
    <div class="todo-main">
        <ul class="todo-list" >
            <ListItem 
                v-for="(item, index) in todos" 
                :todoitem='item' 
                :index='index' 
                :key="item.id"
                :delTodo="delTodo"
                :updateState="updateState"/>
        </ul>
    </div>
</template>

<script lang="ts">
    import { defineComponent, reactive } from 'vue';
    import ListItem from "@/components/ListItem.vue";
    import { Todo } from "@/types/todo";
    export default defineComponent({
        name: "List",
        props: {
            todos: {
                type: Array as () => Todo [],
                required: true
            },
            delTodo: {
                type: Function,
                required: true
            },
            updateState: {
                type: Function,
                required: true
            }
        },
        setup() {

            return {
                

            }
        },
        components:{
            ListItem
        }
    })
</script>

<style scoped>
    .todo-list{
        width: 100%;
    }
</style>