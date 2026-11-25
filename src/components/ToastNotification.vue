<template>
  <transition name="toast">
    <div v-if="visible" class="toast">{{ text }}</div>
  </transition>
</template>

<script setup>
defineProps({
  visible: Boolean,
  text: { type: String, default: 'Спасибо за отзыв!' }
})
</script>

<style scoped>
.toast {
  position: fixed; bottom: 24px; left: 50%; transform: translateX(-50%);
  background: #323232; color: #fff; padding: 14px 24px; border-radius: 8px;
  font-size: 16px; z-index: 200; box-shadow: 0 4px 12px rgba(0,0,0,.3);
}
.toast-enter-active, .toast-leave-active { transition: opacity .3s, transform .3s; }
.toast-enter-from, .toast-leave-to { opacity: 0; transform: translate(-50%, 20px); }
</style>