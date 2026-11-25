<template>
  <div class="overlay" @click.self="close">
    <div class="modal" role="dialog" aria-modal="true">
      <button class="close-btn" @click="close" aria-label="Закрыть">×</button>

      <h2>Оценка мероприятия</h2>

      <!-- Звёзды -->
      <div class="stars" role="radiogroup" aria-label="Оценка">
        <span
          v-for="n in 5"
          :key="n"
          class="star"
          :class="{ active: n <= rating }"
          role="radio"
          :aria-checked="n === rating"
          tabindex="0"
          @click="rating = n"
          @keydown.enter="rating = n"
        >★</span>
      </div>
      <p v-if="error" class="error">{{ error }}</p>

      <!-- Комментарий -->
      <label class="label" for="comment">Комментарий</label>
      <textarea
        id="comment"
        v-model="comment"
        maxlength="500"
        rows="4"
        placeholder="Ваши впечатления..."
      ></textarea>
      <small class="counter">{{ comment.length }} / 500</small>

      <button class="submit-btn" @click="submit">Отправить отзыв</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from 'vue'

const emit = defineEmits(['close', 'sent'])

const rating = ref(0)
const comment = ref('')
const error = ref('')

function validate() {
  error.value = ''
  if (!rating.value) {
    error.value = 'Пожалуйста, поставьте оценку'
    return false
  }
  if (comment.value.length > 500) {
    error.value = 'Комментарий не должен превышать 500 символов'
    return false
  }
  return true
}

function submit() {
  if (!validate()) return
  // «Сохранение» в localStorage — без реальной отправки на сервер
  const feedback = { rating: rating.value, comment: comment.value, date: new Date().toISOString() }
  const all = JSON.parse(localStorage.getItem('feedbacks') || '[]')
  all.push(feedback)
  localStorage.setItem('feedbacks', JSON.stringify(all))

  emit('sent')
}

function close() {
  emit('close')
}

// Закрытие по Esc
function onKeydown(e) {
  if (e.key === 'Escape') close()
}
onMounted(() => window.addEventListener('keydown', onKeydown))
onBeforeUnmount(() => window.removeEventListener('keydown', onKeydown))
</script>

<style scoped>
.overlay {
  position: fixed; inset: 0; background: rgba(0,0,0,.5);
  display: flex; align-items: center; justify-content: center; z-index: 100;
}
.modal {
  background: #fff; border-radius: 12px; padding: 24px;
  width: 90%; max-width: 480px; max-height: 90vh; overflow-y: auto;
  position: relative; box-sizing: border-box;
}
.close-btn {
  position: absolute; top: 8px; right: 12px; background: none; border: none;
  font-size: 28px; cursor: pointer; line-height: 1;
}
.stars { font-size: 36px; user-select: none; margin: 8px 0; }
.star { color: #ccc; cursor: pointer; margin-right: 4px; }
.star.active { color: #f5b301; }
.error { color: #d32f2f; font-size: 14px; margin: 4px 0; }
.label { display: block; margin: 12px 0 4px; font-weight: bold; }
textarea {
  width: 100%; padding: 10px; font-size: 16px; box-sizing: border-box;
  border: 1px solid #ccc; border-radius: 8px; resize: vertical;
}
.counter { color: #888; font-size: 12px; }
.submit-btn {
  margin-top: 16px; width: 100%; padding: 14px; font-size: 16px;
  background: #2b6cb0; color: #fff; border: none; border-radius: 8px; cursor: pointer;
}

/* Мобильная адаптация 390×844 */
@media (max-width: 480px) {
  .modal {
    width: 100%; max-width: 100%; height: 100%;
    max-height: 100%; border-radius: 0; padding: 20px;
  }
  .stars { font-size: 44px; }
  .submit-btn { padding: 16px; font-size: 18px; }
  textarea { font-size: 18px; }
}
</style>