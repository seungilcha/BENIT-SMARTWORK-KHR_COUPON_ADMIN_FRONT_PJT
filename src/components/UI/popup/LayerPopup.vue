<template>
  <div v-if="isVisible" class="layer-popup layer-popup01">
    <div class="popup-content popup-content01">
      <div class="popup-header">
        <h2>{{ title }}</h2>
        <button class="close-btn" @click="closePopup">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 30 30"
            fill="none"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M21.7216 21.7215C22.0928 21.3503 22.0928 20.7484 21.7216 20.3772L16.3444 15L21.7216 9.62276C22.0928 9.25154 22.0928 8.64967 21.7216 8.27845C21.3504 7.90723 20.7485 7.90723 20.3773 8.27845L15 13.6557L9.62275 8.27842C9.25153 7.90719 8.64965 7.90719 8.27843 8.27842C7.90721 8.64964 7.90721 9.25151 8.27843 9.62273L13.6557 15L8.27842 20.3773C7.90719 20.7485 7.90719 21.3503 8.27842 21.7216C8.64964 22.0928 9.25151 22.0928 9.62274 21.7216L15 16.3443L20.3773 21.7215C20.7485 22.0928 21.3504 22.0928 21.7216 21.7215Z"
              fill="#1C1C1C"
            />
          </svg>
        </button>
      </div>
      <div class="popup-body">
        <p v-html="message"></p>
      </div>
      <div class="popup-footer">
        <BaseButton
          label="닫기"
          type="default"
          size="large"
          @click="closePopup"
          variant="secondary"
        />
        <BaseButton
          label="삭제"
          type="primary"
          size="large"
          @click="closePopup"
          variant="secondary"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";
import BaseButton from "../BaseButton.vue";

defineProps({
  isVisible: { type: Boolean, required: true },
  title: { type: String, required: true },
  message: { type: String, required: true },
});

const emit = defineEmits(["close", "confirm"]);

const closePopup = () => {
  emit("close");
};
</script>

<style scoped>
.layer-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-content {
  background: var(--white-color);
  padding: 30px;
  width: 428px;
  border: 1px solid var(--fontbk-color);
}

.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 15px;
}

.popup-header h2 {
  font-size: var(--font-size-xl);
  color: var(--fontbk-color);
}

.popup-body {
  padding: 0px 0px 20px 0px;
}
.popup-body p {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
}

.popup-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding-top: 10px;
}

.popup-footer button {
  width: 100%;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
}
</style>
