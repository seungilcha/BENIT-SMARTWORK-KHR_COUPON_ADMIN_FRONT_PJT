<template>
  <div class="app-container">
    <!-- 🏠 상단 헤더 -->
    <LayoutHeader />

    <div class="main-content">
      <!-- 📌 네비게이션 영역 (사이드바) -->
      <LayoutNav @update-content="updateContent" />

      <!-- 📌 콘텐츠 영역 (선택된 페이지 표시) -->
      <ContentArea
        :folderName="selectedFolder"
        :componentName="selectedComponent"
      />
    </div>
  </div>
</template>

<script>
import LayoutHeader from "@/components/Layout/LayoutHeader.vue";
import LayoutNav from "@/components/Layout/LayoutNav.vue";
import ContentArea from "@/components/Layout/ContentArea.vue";

export default {
  components: {
    LayoutHeader,
    LayoutNav,
    ContentArea,
  },
  data() {
    return {
      selectedFolder: "SysMgmt", // 📂 기본 폴더명 (초기값: 시스템 관리)
      selectedComponent: "page_Admin", // 📄 기본 표시할 페이지 (초기값: 관리자 관리)
    };
  },
  methods: {
    /**
     * 네비게이션에서 선택한 페이지를 업데이트하는 함수
     * @param {Object} param - 폴더명(folderName)과 컴포넌트명(componentName)
     */
    updateContent({ folderName, componentName }) {
      this.selectedFolder = folderName;
      this.selectedComponent = componentName;
    },
  },
};
</script>

<style scoped>
/* 📌 전체 레이아웃 (화면을 100vh로 유지) */
.app-container {
  display: flex;
  flex-direction: column;
  height: 100vh;
}

/* 📌 메인 콘텐츠 (네비게이션 + 콘텐츠 영역 포함) */
.main-content {
  display: flex;
  flex-grow: 1;
}
</style>
