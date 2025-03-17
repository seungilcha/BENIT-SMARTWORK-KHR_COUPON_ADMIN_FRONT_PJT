<template>
  <div class="page-container">
    <!-- 상단 헤더 컴포넌트 -->
    <AdminHeader
      title="관리자 등록/관리"
      :buttons="tableHeaderButtons"
      @button-click="handleButtonClick"
    />
  </div>
</template>

<script setup>
import { computed } from "vue";
import AdminHeader from "../../UI/AdminHeader.vue";

// 테이블 헤더 버튼 정의
const tableHeaderButtons = computed(() => [
  {
    label: "등록",
    type: "danger",
    size: "large",
  },
]);
</script>

<style scoped>
.page-container {
  display: block;
}
</style>
