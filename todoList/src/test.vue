<template>
<div>
    <input type="text" v-model='number'/>
    <input type="text" v-model='message' :value='getcount()'/>
</div>
    
</template>
<script>
import AsyncComponent from './AsyncComponent.vue'
export default {
   setup(props,ctx) {
    const message = computed({
      get: function() {
        return remaining.value === 0;
      },
      set: function(value) {
        state.todos.forEach(function(todo) {
          todo.completed = value;
        });
      }
    });
return {
       allDone
   };
   }
   
}
</script>