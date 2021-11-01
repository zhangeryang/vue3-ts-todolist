<template>
	<div class="todo-wrap">
		<h2>todoList</h2>
		<Header :addToPlan='addToPlan'></Header>
		<List :todos='todos' :delTodo="delTodo" :updateState="updateState"></List>
		<Footer :todos='todos'
				:clearCompleted='clearCompleted'
				:checkAll='checkAll'/>
	</div>
</template>

<script lang="ts">
import { defineComponent, provide, reactive, toRefs, watch, onMounted } from 'vue';
import Header from './components/Head.vue'
import List from './components/List.vue'
import Footer from './components/Footer.vue'
import { Todo } from "@/types/todo";
import { saveTodos, readTodos } from "@/utils/localStorageUtils";

export default defineComponent({
	name: 'App',
	components: {
		Header,
		List,
		Footer
	},
	setup() {
		// const state = reactive<{todos:Todo[]}>({
		//   todos:[
		//     {id:1,title:'xxx',isCompleted:false},
		//     {id:2,title:'xxx',isCompleted:true},
		//     {id:3,title:'xxx',isCompleted:false}
		//   ]
		// })

		// 定义一个state对象，里面是 todos 数组
		const state = reactive<{todos:Todo[]}>({
			todos:[]
		})

		//界面加载完毕后再从缓存中读取数据
		onMounted(()=> {
			setTimeout(() => { state.todos = readTodos() }, 1000)
		})

		// 监听 todos
		// value=>saveTodos(value)缩写
		watch(() => state.todos, saveTodos, { deep:true })


		// 用于head组件添加数据
		const addToPlan = (todo:Todo) => {
			state.todos.unshift(todo)
		}

		// 用于head组件删除数据
		const delTodo:Function = (index:number) => {
			state.todos.splice(index, 1)
		}

		// 改变状态
		const updateState = (todo:Todo, val:boolean) => {
			todo.isCompleted = val
			console.log(todo)
		}

		// 全选 / 反选
		const checkAll = (val:boolean) => {
			state.todos.forEach(item => {
				item.isCompleted = val
			})
		}

		// 计算属性
		const clearCompleted = ()=> {
			state.todos = state.todos.filter((item,index)=>{
				if(item.isCompleted) {
					return false
				}
				return true
			})
		}


		// provide('delTodo', delTodo)
		// provide('updateState', updateState)

		return{
			// 将state中的所有属性转换为ref对象 ，用的时候直接使用属性名称即可
			...toRefs(state),
			// 给header组件的添加方法
			addToPlan,
			// 给footer组件的全选方法
			checkAll,
			clearCompleted,
			delTodo,
			updateState
		}
	}

	
});
</script>

<style>

  .todo-wrap {
    width: 500px;
    margin: 0 auto;
    border: 1px solid lightblue;
    border-radius: 5px;
    padding: 20px;
  }
  .todo-wrap h2 {
    text-align: center;
  }
</style>
