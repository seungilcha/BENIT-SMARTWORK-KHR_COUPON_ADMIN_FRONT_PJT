<template>
  <div class="table-content">
    <table>
      <thead>
        <tr>
          <th>
            <BaseToggle v-model="allChecked" variant="square" />
          </th>
          <th v-for="(header, index) in headers.slice(1)" :key="index">
            {{ header }}
          </th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(row, rowIndex) in rows" :key="rowIndex">
          <td>
            <BaseToggle v-model="row.checked" />
          </td>
          <td v-for="(cell, cellIndex) in row.data" :key="cellIndex">
            <template v-if="Array.isArray(cell)">
              <div class="button-group">
                <BaseButton
                  v-for="(item, btnIndex) in cell"
                  :key="btnIndex"
                  :label="item.label"
                  @click="item.action"
                />
              </div>
            </template>
            <template v-else-if="cell.type === 'text'">
              {{ cell.value }}
            </template>
            <template v-else-if="cell.type === 'button'">
              <BaseButton :label="cell.label" @click="cell.action" />
            </template>
          </td>
        </tr>
        <tr v-if="rows.length === 0">
          <td :colspan="headers.length" class="no-data">데이터가 없습니다.</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { defineProps, computed } from "vue";
import BaseButton from "../BaseButton.vue";
import BaseToggle from "../BaseToggle.vue";

const props = defineProps({
  data: Array, // ✅ 부모 컴포넌트에서 데이터를 받아옴
  headers: Array, // 테이블 헤더 (th)
  rows: Array, // 테이블 행 (td + 체크박스)
});
// ✅ headers와 rows가 `undefined`인지 확인
const headers = computed(() => props.headers || []);
const rows = computed(() => props.rows || []);

const allChecked = computed({
  get: () => rows.value.every((row) => row.checked),
  set: (value) => {
    rows.value.forEach((row) => (row.checked = value));
  },
});
</script>

<style scoped>
.content-area {
  padding: 0px 35px 35px 35px;
} /* 테이블 및 페이지네이션 스타일 */
.table-content table {
  width: 100%;
  border-collapse: collapse;
  border-top: 1px solid var(--Gray-color);
  border-bottom: 1px solid var(--Gray-color);
}

th {
  background-color: var(--LightGray-color);
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid var(--lineStroke-color);
}

td {
  padding: 12px;
  text-align: left;
  border-top: 1px solid var(--lineStroke-color);
}

.button-group {
  display: flex;
  gap: 5px;
}
.selected-row {
  background-color: var(--secondary-color); /* 연한 초록색 */
  transition: background-color 0.2s ease;
}

.inactive-row {
  color: var(
    --GrayHintText-color
  ) !important; /* 🔴 "미사용"이 포함된 행의 텍스트 색상을 빨간색으로 변경 */
}
</style>
