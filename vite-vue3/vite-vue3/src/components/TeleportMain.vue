<template>
<div class="confirm-modal">
    <button @click="isOpen = true">打开{{isOpen}} </button>
    <!-- 注意这一块代码 teleport -->
    <teleport to="#foot-container">
        <div v-if="isOpen">
          <p>提示提示提示提示提示提示</p>
          <button @click="isOpen = false">取消</button>
        </div>
    </teleport>
</div>
</template>

<script>
import {ref} from 'vue'
export default {
    setup(){
        const isOpen = ref(false)
        return {isOpen}
    }
}
</script>