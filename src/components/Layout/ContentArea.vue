<template>
  <div class="content-area">
    <!-- 📌 동적으로 로드된 컴포넌트 표시 -->
    <component :is="currentComponent" v-if="currentComponent" />
  </div>
</template>

<script setup>
import {
  shallowRef,
  watch,
  defineAsyncComponent,
  markRaw,
  defineProps,
} from "vue";

const props = defineProps(["folderName", "componentName"]);
const currentComponent = shallowRef(null);

const modules = import.meta.glob("/src/components/pages/**/*.vue");

const loadComponent = async (folder, component) => {
  if (!folder || !component) return;

  const filePath = `/src/components/pages/${folder}/${component}.vue`;

  if (modules[filePath]) {
    currentComponent.value = markRaw(defineAsyncComponent(modules[filePath]));
  }
};

// ✅ Props 변경 감지 → 동적으로 컴포넌트 로드
watch(
  () => [props.folderName, props.componentName],
  ([newFolder, newComponent]) => {
    loadComponent(newFolder, newComponent);
  },
  { immediate: true }
);
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
