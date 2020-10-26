<template>
<section></section>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3.0 + Vite" /> -->
  <TeleportMain/>
  <!-- 注意注意异步组件 -->
<MainAsync/>
  <div></div>

</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import MainAsync from './components/MainAsync.vue'
import TeleportMain from './components/TeleportMain.vue'
import A from './a.js'
import {ref} from 'vue'
export default {
  name: 'App',
  components: {
    HelloWorld,
    MainAsync,
    TeleportMain
  }
  //  setup(){
  //       const isOpen = ref(false)
  //       return {isOpen}
  //   }
}
</script>
