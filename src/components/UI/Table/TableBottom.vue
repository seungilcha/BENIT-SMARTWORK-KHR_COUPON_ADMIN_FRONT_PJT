<template>
  <div class="table-bottom pagination-container">
    <!-- 📌 페이지네이션 -->
    <div class="pagination">
      <button
        @click="emit('prevPage')"
        :disabled="currentPage === 1"
        v-html="prevIcon"
      ></button>

      <button
        v-for="page in displayedPages"
        :key="page"
        @click="emit('goToPage', page)"
        :class="{ active: page === currentPage }"
      >
        {{ page }}
      </button>

      <button
        @click="emit('nextPage')"
        :disabled="currentPage === totalPages"
        v-html="nextIcon"
      ></button>
    </div>

    <!-- 📌 행 개수 선택 -->
    <div class="row-selector">
      <BaseInput
        type="select"
        :options="options"
        v-model="selectedRowsPerPage"
        @update:modelValue="handleChange"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineProps, defineEmits } from "vue";
import BaseInput from "../BaseInput.vue";

const props = defineProps({
  currentPage: Number,
  totalPages: Number,
  rowsPerPage: Number,
});

const emit = defineEmits([
  "prevPage",
  "nextPage",
  "goToPage",
  "updateRowsPerPage",
]);

// **SVG 아이콘**
const prevIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" fill="none">
  <path d="M11.2426 14.2426L7 10L11.2426 5.75736" stroke="#1C1C1C" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
</svg>`;

const nextIcon = `<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" fill="none">
  <path d="M8.75736 14.2426L13 10L8.75736 5.75736" stroke="#1C1C1C" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
</svg>`;

// **최대 5개의 페이지 버튼만 표시**
const displayedPages = computed(() => {
  const range = 2; // 현재 페이지 앞뒤로 2개씩 보이게 함
  let start = Math.max(1, props.currentPage - range);
  let end = Math.min(props.totalPages, props.currentPage + range);

  return Array.from({ length: end - start + 1 }, (_, i) => start + i);
});

// **행 개수 선택 옵션**
const options = [
  { value: 10, label: "10개씩 보기" },
  { value: 20, label: "20개씩 보기" },
  { value: 30, label: "30개씩 보기" },
];

const selectedRowsPerPage = ref(props.rowsPerPage || 10);

// **행 개수 변경 시 부모 컴포넌트에 전달**
const handleChange = (value) => {
  selectedRowsPerPage.value = value;
  emit("updateRowsPerPage", value);
};
</script>

<style scoped>
.table-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
  padding: 0px 30px 10px 30px;
  border-bottom: 1px solid var(--lineStroke-color);
}

/* ✅ 페이지네이션 컨테이너 정렬 */
.pagination-container {
  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-grow: 1;
}

/* ✅ 버튼 및 숫자 버튼 일렬 정렬 */
.pagination {
  display: flex;
  align-items: center;
  gap: 8px; /* 버튼 간격 조정 */
}

.pagination button {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  height: 20px;
  line-height: 20px;
  font-size: var(--font-size-base);
  align-items: center;
  color: var(--GrayHintText-color);
}
.pagination button.active {
  color: var(--fontbk-color);
}
/* ✅ 비활성화 버튼 스타일 */
.pagination button:disabled {
  opacity: 0.2;
  cursor: default;
}
.pagination button span {
  height: 20px;
}
/* ✅ 행 개수 선택 */
.row-selector {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-left: auto;
}
</style>
