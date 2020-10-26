<template>
  <section class="todoapp">
    <!-- v-cloak 闪烁问题-->
     <header class="header">
        <h1>Vue3 todos</h1>
        <input class="new-todo"
          placeholder="What needs to be done?"
          v-model="newTodo"
          @keyup.enter="addTodo">
      </header>
      <section class="main" v-cloak>
        <input class="toggle-all" type="checkbox" v-model="allDone">
        <ul class="todo-list">
          <li v-for="todo in todos"
            class="todo"
            :key="todo.id"
            :class="{ completed: todo.completed, editing: todo == editedTodo }"
            >
            <div class="view">
              <input class="toggle" type="checkbox" v-model="todo.completed">
              <label @dblclick="editTodo(todo)">{{ todo.title }}</label>
              <button class="destroy" @click="removeTodo(todo)"></button>
            </div>
          </li>
        </ul>
      </section>
       <footer class="footer" v-show="todos.length" v-cloak>
        <span class="todo-count">
          <strong>{{ remaining }}</strong>  left
        </span>
        <button class="clear-completed" @click="removeCompleted" v-show="todos.length > remaining">
          Clear completed
        </button>
      </footer>
  </section>
  
</template>

<script>
// setup是新的选项，可以理解是comopsition的入口，函数内部在beforeCreate之前调用，函数返回的内容会作为模板渲染的上下文
// reactive 创建一个响应式状态  响应式状态的基本用例就是在渲染时使用它
// watchEffect 渲染内容 响应式状态的副作用，并自动进行重应用
// toRefs 是将响应式对象转化成普通
// ref 和 reactive reactive负责复杂数据结构，ref可以把基本的数据结构包装成响应式
// watch  和vue 2 一样
// effect副作用函数，相关值变化，就会执行该函数
// computed 和 vue2 一样
// 关于data、method、computed，vue2里的都是挂载在this之上 有两个明显的缺点 没有使用到computed 也会被打包 有利于tree-shaking
// option API  和 comopsition API ，后者vue3 可以避免修改时候上下横跳来回改值（mixin数据来源不清）  后者在开发复杂组件，大组件有明显维护开发有优势
//comopsition API ==  reactive + 生命周期
// 关于生命周期  只能在setup 中调用  
// 组件实例上下文也是在生命周期钩子同步执行期间设置的，因此，在卸载组件时，在生命周期钩子内部同步创建的侦听器和计算状态也将自动删除。
// beforeCreate -> 使用 setup()
// created -> 使用 setup()
// beforeMount -> onBeforeMount
// mounted -> onMounted
// beforeUpdate -> onBeforeUpdate
// updated -> onUpdated
// beforeDestroy -> onBeforeUnmount
// destroyed -> onUnmounted
// errorCaptured -> onErrorCaptured




// 关于 新的组件特性
// 新的组件Fragment, Teleport, Suspense
// 1.Fragment  很简单 不需要根节点 减少了 不必要的div  组件递归实现了平级递归
// 2.Teleport （翻译 传送）  就是vue渲染到指定节点上，用在弹窗操作比较多(原因：因为弹窗需要渲染到最外层body下面，否则嵌套过多，蒙层可能会被父元素的transform影响)
// 3.Suspense（翻译 悬念、异步） 更好的实现异步组件的方式 据说：（ suspense和vue-router配合 ，以及报错处理用处比较大，没用过）

// import HelloWorld from './components/HelloWorld.vue'
import useAddRemove from './addRemove'
import { reactive, toRefs, computed} from "vue"
export default {
  
  // setup是新的选项，可以理解是comopsition的入口，函数内部在beforeCreate之前调用，函数返回的内容会作为模板渲染的上下文
  // 替代了beforeCreate 、Create 参数是props 替代了Props ctx 就是this 在 setup() 函数中无法访问到 this
  setup(props,ctx) {
    console.log(props,ctx);
    const state = reactive({
      todos: [
        {
          id: "1",
          title: "吃饭",
          completed: false
        },
        {
          id: "2",
          title: "睡觉",
          completed: false
        }
      ],
      newTodo: "",
      editedTodo: null
    });
    const remaining = computed(
      () => state.todos.filter(todo => !todo.completed).length
    );
    const allDone = computed({
      get: function() {
        return remaining.value === 0;
      },
      set: function(value) {
        state.todos.forEach(function(todo) {
          todo.completed = value;
        });
      }
    });

    function editTodo(todo) {
      console.log('tag', todo);
      // state.beforeEditCache = todo.title;
      // state.editedTodo = todo;
    }
    function removeCompleted() {
      state.todos = state.todos.filter(todo => todo.completed);
    }
    const {addTodo,removeTodo} = useAddRemove(state)
    return {
      ...toRefs(state),
      // addTodo,removeTodo,
      remaining,allDone,
      addTodo,removeTodo,editTodo,removeCompleted
  };
}
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
