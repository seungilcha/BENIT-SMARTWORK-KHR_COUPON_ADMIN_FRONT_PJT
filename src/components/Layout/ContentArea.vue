<template>
  <div class="content-area">
    <!-- 📌 동적으로 로드된 컴포넌트 표시 -->
    <component v-if="loadedComponent" :is="loadedComponent" />

    <!-- ⏳ 컴포넌트 로딩 중 표시 -->
    <div v-else class="loading">로딩 중...</div>
  </div>
</template>

<script>
import { defineAsyncComponent, markRaw } from "vue";

export default {
  props: {
    folderName: String, // 📂 폴더명 (예: 'SysMgmt' 또는 'SiteMgmt')
    componentName: String, // 📄 Vue 파일명 (예: 'page_Admin')
  },
  data() {
    return {
      loadedComponent: null, // 🔹 동적으로 로드할 컴포넌트 저장
    };
  },
  watch: {
    /**
     * componentName이 변경될 때 Vue 파일을 동적으로 로드
     * @param {String} newComponent - 새로 변경된 컴포넌트명
     * @param {String} oldComponent - 이전 컴포넌트명
     */
    async componentName(newComponent, oldComponent) {
      if (newComponent && newComponent !== oldComponent) {
        await this.loadComponent(this.folderName, newComponent);
      }
    },
  },
  async created() {
    // 📌 최초 로딩 시 기본 페이지 (`page_Admin.vue`) 표시
    await this.loadComponent(
      this.folderName || "SysMgmt",
      this.componentName || "page_Admin"
    );
  },
  methods: {
    /**
     * Vue 파일을 비동기적으로 로드하는 함수
     * @param {String} folder - 폴더명 (예: 'SysMgmt')
     * @param {String} page - 컴포넌트 파일명 (예: 'page_Admin')
     */
    async loadComponent(folder, page) {
      try {
        // 📌 defineAsyncComponent 사용하여 동적 임포트
        const component = defineAsyncComponent(
          () => import(`@/components/pages/${folder}/${page}.vue`)
        );

        // ✅ markRaw 적용하여 반응형 문제 방지 (Vue3 반응성 처리 최적화)
        this.loadedComponent = markRaw(component);
      } catch (error) {
        console.error(`🚨 [오류] ${folder}/${page}.vue 로드 실패`, error);
        this.loadedComponent = null; // ⚠ 오류 발생 시 컴포넌트 비우기
      }
    },
  },
};
</script>

<style scoped>
/* 📌 콘텐츠 영역 스타일 */
.content-area {
  flex-grow: 1;
  min-height: calc(100vh - 62px);
  overflow: auto;
  min-width: 916px;
}

/* ⏳ 로딩 메시지 스타일 */
.loading {
  text-align: center;
  font-size: 16px;
  color: var(--font-size-lg);
}
</style>
