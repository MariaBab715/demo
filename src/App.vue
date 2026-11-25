<template>
  <div class="page">
    <h1>Заявки на конференцию</h1>
    <p>Мероприятие завершено. Оставьте, пожалуйста, отзыв.</p>

    <!-- Кнопка открытия формы -->
    <button v-if="!feedbackSent" class="open-btn" @click="isModalOpen = true">
      Оставить отзыв
    </button>

    <!-- Текст после отправки -->
    <p v-else class="sent-text">Ваш отзыв отправлен</p>

    <!-- Модальное окно -->
    <FeedbackModal
      v-if="isModalOpen"
      @close="isModalOpen = false"
      @sent="onSent"
    />

    <!-- Toast -->
    <ToastNotification :visible="toastVisible" text="Спасибо за отзыв!" />
  </div>
</template>

<script setup>
import { ref } from 'vue'
import FeedbackModal from './components/FeedbackModal.vue'
import ToastNotification from './components/ToastNotification.vue'

const isModalOpen = ref(false)
const toastVisible = ref(false)
const feedbackSent = ref(false)

function onSent() {
  isModalOpen.value = false
  feedbackSent.value = true
  toastVisible.value = true
  setTimeout(() => (toastVisible.value = false), 3000)
}
</script>

<style>
.page { max-width: 800px; margin: 40px auto; padding: 0 16px; font-family: sans-serif; }
.open-btn { padding: 12px 24px; font-size: 16px; cursor: pointer; }
.sent-text { color: green; font-weight: bold; font-size: 18px; }
</style>