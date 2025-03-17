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
            fill="none"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M21.722 21.722a1 1 0 0 0 0-1.414L16.344 15l5.378-5.379a1 1 0 0 0-1.414-1.414L15 13.586 9.621 8.207a1 1 0 1 0-1.414 1.414L13.586 15l-5.379 5.378a1 1 0 0 0 1.414 1.414L15 16.414l5.378 5.308a1 1 0 0 0 1.344 0z"
              fill="#1C1C1C"
            />
          </svg>
        </button>
      </div>
      <div class="popup-body">
        <button class="download-template-btn" @click="downloadTemplate">
          <img src="../../../images/icons8-ms-excel.svg" alt="Excel-icon" />양식
          다운로드
        </button>

        <div class="upload-area">
          <input
            ref="fileInput"
            type="file"
            class="file-input"
            @change="handleFileChange"
            style="display: none"
          />
          <input
            type="text"
            :value="fileName"
            placeholder="파일을 첨부하세요."
            class="file-name-input"
            readonly
          />
          <BaseButton
            @click="triggerFileUpload"
            type="default"
            size="small"
            class="attachment"
          >
            첨부파일
          </BaseButton>
        </div>
      </div>
      <div class="popup-footer">
        <BaseButton
          @click="closePopup"
          type="table-header-default"
          size="small"
        >
          닫기
        </BaseButton>
        <BaseButton
          @click="confirmUpload"
          :type="isFileUploaded ? 'primary' : 'danger'"
          size="small"
        >
          업로드
        </BaseButton>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import BaseButton from "../BaseButton.vue";

const isVisible = ref(false);
const fileInput = ref(null);
const fileName = ref("");
const uploadedFile = ref(null);

const { title } = defineProps({
  title: { type: String, default: "엑셀 업로드" },
});

const emit = defineEmits(["upload"]);

const isFileUploaded = ref(false);

const openPopup = () => {
  isVisible.value = true;
};

const closePopup = () => {
  isVisible.value = false;
  fileName.value = "";
  uploadedFile.value = null;
  isFileUploaded.value = false;
};

const triggerFileUpload = () => {
  fileInput.value.click();
};

const handleFileChange = (event) => {
  const file = event.target.files[0];
  if (file) {
    fileName.value = file.name;
    uploadedFile.value = file;
    isFileUploaded.value = true;
  }
};

const downloadTemplate = () => {
  const link = document.createElement("a");
  link.href = "/path/to/excel/template.xlsx"; // 양식 다운로드 경로를 수정하세요.
  link.download = "관리자 명단_v2.0_20250101.xlsm";
  link.click();
};

const confirmUpload = () => {
  if (!isFileUploaded.value) return;
  emit("upload", uploadedFile.value);
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
  width: 368px;
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

.download-template-btn {
  border-radius: 4px;
  border: 1px solid var(--lineStroke-color);
  background: var(--white-color);
  cursor: pointer;
  font-weight: 500;
  font-size: var(--font-size-base);
  padding: 7.5px 20px 7.5px 17px;
  margin-bottom: 10px;
}
.download-template-btn img {
  display: inline-block;
  vertical-align: sub;
  margin-right: 5px;
}

.upload-area {
  display: flex;
  gap: 10px;
  align-items: center;
  background: var(--secondary-color);
}

.file-name-input {
  flex: 1;
  padding: 0px 0px 0px 20px;
  background: var(--secondary-color);
  border: none;
}
.file-name-input:focus {
  outline: 0px solid var(--lineStroke-color);
}
.file-name-input::placeholder {
  color: var(--GrayHintText-color);
}
.popup-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  margin-top: 10px;
}

.attachment {
  border-radius: 4px;
  border: 1px solid var(--lineStroke-color);
  background: var(--white-color);
  padding: 5px 15px;
  font-size: var(--font-size-sm);
  margin: 10px 15px;
}

.close-btn {
  background: none;
  border: none;
  cursor: pointer;
}
</style>
