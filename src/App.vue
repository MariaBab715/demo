<template>
  <div class="page-wrapper">
    <div class="content-card">
      <h1 class="title">Заявки на конференцию</h1>
      <p class="subtitle">Мероприятие завершено. Мы будем рады узнать ваше мнение!</p>

      <!-- Кнопка открытия формы -->
      <transition name="fade" mode="out-in">
        <button v-if="!feedbackSent" key="btn" class="open-btn" @click="isModalOpen = true">
          <span>Оставить отзыв</span>
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"></path></svg>
        </button>

        <!-- Текст после отправки -->
        <div v-else key="sent" class="sent-badge">
          <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="20 6 9 17 4 12"></polyline></svg>
          Ваш отзыв отправлен
        </div>
      </transition>
    </div>

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
/* Глобальные стили для фона страницы */
body {
  margin: 0;
  background-color: #f4f7f9; /* Мягкий серо-голубой фон */
  font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  color: #2d3748;
}

.page-wrapper {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  box-sizing: border-box;
}

.content-card {
  background: #ffffff;
  padding: 40px;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.05);
  max-width: 600px;
  width: 100%;
  text-align: center;
}

.title {
  font-size: 28px;
  font-weight: 700;
  margin: 0 0 12px 0;
  color: #1a202c;
}

.subtitle {
  font-size: 16px;
  color: #718096;
  margin: 0 0 32px 0;
  line-height: 1.5;
}

/* Стильная кнопка */
.open-btn {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 28px;
  font-size: 16px;
  font-weight: 600;
  color: #ffffff;
  background: linear-gradient(135deg, #4f46e5 0%, #7c3aed 100%);
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  box-shadow: 0 4px 15px rgba(79, 70, 229, 0.3);
}

.open-btn:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(79, 70, 229, 0.4);
}

.open-btn:active {
  transform: translateY(0);
}

/* Бейдж "Отправлено" */
.sent-badge {
  display: inline-flex;
  align-items: center;
  gap: 8px;
  padding: 14px 28px;
  font-size: 16px;
  font-weight: 600;
  color: #059669;
  background: #ecfdf5;
  border: 1px solid #a7f3d0;
  border-radius: 12px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>