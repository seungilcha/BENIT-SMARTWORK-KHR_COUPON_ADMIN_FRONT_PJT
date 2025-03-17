<template>
  <div class="table-main">
    <!-- 테이블 헤더 -->
    <TableHeader
      :title="title"
      :tableHeaderButtons="tableHeaderButtons || []"
      :totalUsers="filteredUsers?.length || 0"
      :tableButtons="tableButtons"
      :unit="unit"
      @buttonClick="handleTableButtonClick"
    />

    <!-- 테이블 내용 -->
    <div>
      <TableContent
        :data="data"
        v-for="(table, index) in tables"
        :key="index"
        :headers="table.headers"
        :rows="table.rows"
      />
    </div>

    <!-- 페이지네이션 및 행 개수 선택 -->
    <TableBottom
      :currentPage="currentPage"
      :totalPages="totalPages"
      :rowsPerPage="rowsPerPage"
      :data="data"
      @updateRowsPerPage="emit('updateRowsPerPage', $event)"
      @prevPage="emit('prevPage')"
      @nextPage="emit('nextPage')"
      @goToPage="emit('goToPage', $event)"
    />
  </div>
</template>

<script setup>
import { defineProps, computed, defineEmits } from "vue";
import TableHeader from "./TableHeader.vue";
import TableContent from "./TableContent.vue";
import TableBottom from "./TableBottom.vue";

const props = defineProps({
  title: String,
  tableHeaderButtons: Array,
  tableButtons: Array,
  unit: { type: String, default: "명" },
  tables: Array, // 여러 개의 테이블 데이터를 담은 배열
  data: Array,
  rowsPerPage: Number,
  totalPages: Number,
  currentPage: Number,
});

const emit = defineEmits([
  "updateRowsPerPage",
  "prevPage",
  "nextPage",
  "goToPage",
  "buttonClick",
]);

// 이벤트를 상위로 전달
const handleTableButtonClick = (label, users) => {
  emit("buttonClick", label, users);
};
// ✅ `tables`에서 `rows`를 모두 합쳐서 `filteredUsers` 계산
const filteredUsers = computed(() => {
  return props.tables?.reduce((acc, table) => acc.concat(table.rows), []) || [];
});
</script>

<style scoped>
.table-main {
  padding: 0px 35px 35px 35px;
}
</style>
