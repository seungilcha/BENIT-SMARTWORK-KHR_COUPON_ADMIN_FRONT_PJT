<template>
  <button :class="buttonClass" :disabled="disabled" @click="$emit('click')">
    <!-- 왼쪽 아이콘 -->
    <slot name="icon"></slot>
    <!-- 버튼 텍스트 -->
    <slot>{{ label }}</slot>
  </button>
</template>

<script>
export default {
  props: {
    label: String, // 버튼 텍스트
    icon: String, // 아이콘 클래스 (왼쪽 아이콘)
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
          "table-header-danger",
        ].includes(value),
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    buttonClass() {
      return [
        "btn",
        `btn-${this.type}`,
        `btn-${this.variant}`,
        `btn-${this.size}`,
        `btn-${this.position}`,
        { disabled: this.disabled },
      ];
    },
  },
};
</script>

<style scoped>
/* 기본 버튼 스타일 */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 12px 20px;
  border: none;
  border-radius: var(--spacing-xs);
  cursor: pointer;
  font-size: var(--font-size-base);
  transition: all 0.2s ease-in-out;
  gap: 5px; /* 아이콘과 텍스트 간격 */
}

/* 버튼 색상 스타일 */
.btn-default {
  background-color: var(--white-color);
  color: var(--fontbk-color);
  border: 1px solid var(--lineStroke-color);
  font-weight: 500;
}

.btn-primary {
  background-color: var(--primary-color);
  color: var(--white-color);
  border: 1px solid var(--primary-color);
}

.btn-danger {
  background-color: var(--LightGray-color);
  color: var(--GrayHintText-color);
  border: 1px solid var(--lineStroke-color);
  cursor: default;
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

/* SVG 아이콘 크기 */
.btn svg {
  width: 14px;
  height: 14px;
  fill: none;
}
</style>
