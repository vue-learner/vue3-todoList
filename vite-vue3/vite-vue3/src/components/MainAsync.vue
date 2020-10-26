<template>
  <h1>Supense</h1>
  <suspense>
    <template #default>
      <AsyncComponent :timeout="3000" />
    </template>
    <template #fallback>
      <h1>加载中</h1>
    </template>
  </suspense>
  <div></div>
</template>

<script>
import AsyncComponent from './AsyncComponent.vue'
export default {
  components:{
    AsyncComponent
  }
}
</script>