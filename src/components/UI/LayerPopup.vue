<template>
  <div v-if="isVisible" class="layer-popup">
    <div class="popup-content">
      <div class="popup-header">
        <h2>{{ title }}</h2>
        <button class="close-btn" @click="closePopup">&times;</button>
      </div>
      <div class="popup-body">
        <p v-html="message"></p>
      </div>
      <div class="popup-footer">
        <BaseButton label="닫기" @click="closePopup" variant="secondary" />
        <BaseButton label="삭제" @click="deleteRows" variant="danger" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import BaseButton from "../BaseButton.vue";

const props = defineProps({
  isVisible: Boolean,
  title: String,
  message: String,
  selectedRows: Array, // ✅ 선택된 행 데이터 받기
  tableData: Array, // ✅ 전체 테이블 데이터 받기
});

const emit = defineEmits(["close", "update-table"]);

const closePopup = () => {
  emit("close");
};

const deleteRows = () => {
  // ✅ 선택된 행을 제외한 데이터로 테이블 업데이트
  const updatedTable = props.tableData[0].rows.filter(
    (row) => !props.selectedRows.includes(row)
  );

  emit("update-table", updatedTable); // ✅ 부모 컴포넌트로 업데이트된 데이터 전달
  emit("close"); // ✅ 팝업 닫기
};
</script>

<style scoped>
.layer-popup {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}

.popup-content {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 400px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #ddd;
  padding-bottom: 10px;
}

.popup-body {
  padding: 20px 0;
}

.popup-footer {
  display: flex;
  justify-content: flex-end;
  gap: 10px;
  padding-top: 10px;
}

.close-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
}
</style>
