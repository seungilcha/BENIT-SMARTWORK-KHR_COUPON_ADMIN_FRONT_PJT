<template>
  <div v-if="isVisible" class="popup-wrapper">
    <div class="popup-layer">
      <div class="popup-header">
        <h3>{{ title }}</h3>
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
        <textarea
          v-model="textareaContent"
          :maxlength="1000"
          class="excel-textarea"
          placeholder="엑셀 다운로드 사유 입력"
        ></textarea>
        <div class="textarea-counter">
          <span class="current-length">{{ textareaContent.length }}</span>
          <span class="max-length">/1,000</span>
        </div>
      </div>
      <div class="popup-footer">
        <BaseButton
          @click="closePopup"
          type="table-header-default"
          size="small"
        >
          취소
        </BaseButton>
        <BaseButton
          @click="confirmDownload"
          :type="textareaContent.length ? 'primary' : 'danger'"
          size="small"
        >
          다운로드
        </BaseButton>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import BaseButton from "../BaseButton.vue"; // BaseButton 경로 확인

const isVisible = ref(false);
const textareaContent = ref("");

const props = defineProps({
  title: { type: String, default: "엑셀 다운로드" },
});

const emit = defineEmits(["confirm"]);

const openPopup = () => {
  isVisible.value = true;
};

const closePopup = () => {
  isVisible.value = false;
  textareaContent.value = "";
};

const confirmDownload = () => {
  if (!textareaContent.value.length) return;
  emit("confirm", textareaContent.value);
  closePopup();
};

defineExpose({ openPopup });
</script>

<style scoped>
.popup-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.popup-layer {
  background: var(--white-color);
  padding: 20px;
  width: 761px;
  border: 1px solid var(--fontbk-color);
}

.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 15px;
}
.popup-header h3 {
  font-size: var(--font-size-xl);
  color: var(--fontbk-color);
}

.popup-body {
  position: relative;
}

.excel-textarea {
  width: 100%;
  height: 120px;
  resize: none;
  padding: 10px 10px 41px 10px;
  border: 1px solid var(--lineStroke-color);
  box-sizing: border-box;
  border-radius: var(--spacing-xs);
}

.excel-textarea:focus {
  outline: 0px solid var(--lineStroke-color);
}

.textarea-counter {
  position: absolute;
  right: 10px;
  bottom: 10px;
  font-size: var(--font-size-base);
}

.current-length {
  color: var(--fontbk-color);
}

.max-length {
  color: var(--Gray-color);
}

.popup-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 10px;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
}
</style>
