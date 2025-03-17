<template>
  <button :class="buttonClass" :disabled="disabled" @click="emit('click')">
    <!-- 아이콘이 있을 때만 표시 -->
    <span v-if="$slots.icon" class="icon-container">
      <slot name="icon"></slot>
    </span>
    <!-- 버튼 텍스트 -->
    <span class="button-text">
      <slot>{{ label }}</slot>
    </span>
  </button>
</template>

<script setup>
import { computed, defineProps, defineEmits } from "vue";

const props = defineProps({
  label: String,
  icon: String,
  type: {
    type: String,
    default: "default",
    validator: (value) =>
      ["default", "primary", "danger", "secondary"].includes(value),
  },
  variant: {
    type: String,
    default: "default",
    validator: (value) => ["default", "logout", "disabled"].includes(value),
  },
  size: {
    type: String,
    default: "medium",
    validator: (value) => ["small", "medium", "large"].includes(value),
  },
  position: {
    type: String,
    default: "default",
    validator: (value) =>
      [
        "default",
        "table-header",
        "table-content",
        "table-header-default",
        "table-header-primary",
      ].includes(value),
  },
  disabled: Boolean,
});

const emit = defineEmits(["click"]);

const buttonClass = computed(() => [
  `btn-${props.type}`,
  `btn-${props.variant}`,
  `btn-${props.size}`,
  `btn-position-${props.position}`,
]);
</script>

<style scoped>
button {
  display: flex;
  align-items: center; /* 세로 중앙 정렬 */
  justify-content: center; /* 전체 정렬 */
  gap: 5px; /* ✅ 아이콘과 텍스트 간격 설정 */
}
/* 버튼 색상 스타일 */
.btn-default {
  background-color: var(--white-color);
  color: var(--fontbk-color);
  border: 1px solid var(--lineStroke-color);
  font-weight: 500;
  border-radius: var(--spacing-xs);
}

.btn-primary {
  background-color: var(--primary-color);
  color: var(--white-color);
  border: 1px solid var(--primary-color);
  border-radius: var(--spacing-xs);
}

.btn-danger {
  background-color: var(--LightGray-color);
  color: var(--GrayHintText-color);
  border: 1px solid var(--lineStroke-color);
  cursor: default;
  border-radius: var(--spacing-xs);
}

.btn-logout:hover {
  color: var(--dark-danger-color);
}

/* 버튼 크기 스타일 */
.btn-medium {
  padding: 8px 20px;
}
.btn-small {
  padding: 7px 20px;
}
.btn-large {
  padding: 12px 20px;
}

/* 테이블 위에서 쓰는 버튼 스타일 */
.btn-table-header-default {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--white-color);
  color: var(--fontbk-color);
  border: 1px solid var(--lineStroke-color);
  font-weight: 500;
}
.btn-table-header-primary {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--primary-color);
  color: var(--white-color);
  border: 1px solid var(--primary-color);
}
.btn-table-header-danger {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--LightGray-color);
  color: var(--GrayHintText-color);
  border: 1px solid var(--lineStroke-color);
  cursor: default;
}

/* 테이블 안에서 쓰는 버튼 스타일 */
.btn-table-content-default {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--white-color);
  color: var(--fontbk-color);
  border: 1px solid var(--lineStroke-color);
  font-weight: 500;
}
.btn-table-content-primary {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--primary-color);
  color: var(--white-color);
  border: 1px solid var(--primary-color);
}
.btn-table-content-danger {
  padding: 5px 20px;
  font-size: var(--font-size-sm);
  background-color: var(--LightGray-color);
  color: var(--GrayHintText-color);
  border: 1px solid var(--lineStroke-color);
  cursor: default;
}

/* 버튼 비활성화 스타일 */
.btn:disabled,
.btn.disabled {
  cursor: default;
}

/* 버튼 호버 효과 */
.btn:hover:not(.disabled) {
  opacity: 0.8;
}

/* 버튼 액티브 효과 */
.btn:active:not(.disabled) {
  transform: scale(0.98);
}

/* 아이콘을 감싸는 컨테이너 */
.icon-container {
  width: 20px;
  height: 17px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: -5px;
}
.button-text {
  display: flex;
  align-items: center; /* ✅ 아이콘이 있을 경우에도 텍스트를 수직 가운데 정렬 */
  justify-content: center;
  height: 100%; /* 버튼 내부에서 높이를 꽉 차게 설정 */
}
</style>
