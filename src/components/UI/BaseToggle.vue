<template>
  <label :class="['base-toggle', size, { checked, disabled }]">
    <input
      :type="type"
      :checked="modelValue"
      :disabled="disabled"
      @change="toggleValue"
      class="hidden-input"
    />
    <span class="toggle-icon">
      <component :is="toggleIcon"></component>
    </span>
    <slot></slot>
  </label>
</template>

<script setup>
import { computed, defineProps, defineEmits, h } from "vue";

const props = defineProps({
  modelValue: Boolean,
  type: { type: String, default: "checkbox" }, // checkbox, radio
  size: { type: String, default: "medium" },
  variant: { type: String, default: "default" }, // default, square
  disabled: { type: Boolean, default: false },
});

const emit = defineEmits(["update:modelValue"]);

const toggleValue = (event) => {
  if (!props.disabled) {
    emit("update:modelValue", event.target.checked);
  }
};

// ✅ 체크박스 & 라디오 버튼 아이콘 설정
const toggleIcon = computed(() => {
  if (props.disabled) {
    return h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
      h("rect", {
        x: "0.5",
        y: "0.5",
        width: "19",
        height: "19",
        rx: "5.5",
        stroke: "#CECECE",
      }),
      h("path", {
        d: "M6 10.2727L8.5 13L14 8",
        stroke: "#CECECE",
        "stroke-width": "1.5",
        "stroke-linecap": "round",
        "stroke-linejoin": "round",
      }),
    ]);
  }

  if (props.type === "radio") {
    // ✅ TD 체크박스 (2. 1) 스타일)
    return props.modelValue
      ? h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
          h("circle", {
            cx: "10",
            cy: "10",
            r: "9.5",
            fill: "white",
            stroke: "#183322",
          }),
          h("circle", { cx: "10", cy: "10", r: "6", fill: "#183322" }),
        ])
      : h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
          h("circle", {
            cx: "10",
            cy: "10",
            r: "9.5",
            fill: "white",
            stroke: "#1C1C1C",
          }),
        ]);
  }

  if (props.variant === "square" && props.modelValue) {
    // ✅ TH 체크박스 (2. 3) 스타일)
    return h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
      h("rect", {
        x: "0.5",
        y: "0.5",
        width: "19",
        height: "19",
        rx: "5.5",
        fill: "white",
        stroke: "#183322",
      }),
      h("rect", {
        x: "4",
        y: "4",
        width: "12",
        height: "12",
        rx: "3",
        fill: "#183322",
      }),
    ]);
  }

  if (props.variant === "default" && props.modelValue) {
    // ✅ TD 체크박스 (2. 2) 스타일)
    return h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
      h("path", {
        d: "M0 6C0 2.68629 2.68629 0 6 0H14C17.3137 0 20 2.68629 20 6V14C20 17.3137 17.3137 20 14 20H6C2.68629 20 0 17.3137 0 14V6Z",
        fill: "#183322",
      }),
      h("path", {
        d: "M6 10.2727L8.5 13L14 8",
        stroke: "white",
        "stroke-width": "1.5",
        "stroke-linecap": "round",
        "stroke-linejoin": "round",
      }),
    ]);
  }

  // ✅ 체크 안된 기본 아이콘 (2. 체크 안된 상태)
  return h("svg", { width: "20", height: "20", viewBox: "0 0 20 20" }, [
    h("rect", {
      x: "0.5",
      y: "0.5",
      width: "19",
      height: "19",
      rx: "5.5",
      fill: "white",
      stroke: "#183322",
    }),
  ]);
});
</script>

<style scoped>
/* ✅ 기본 스타일 */
.base-toggle {
  display: flex;
  align-items: center;
  cursor: pointer;
  font-size: 14px;
}

/* ✅ 숨겨진 기본 input */
.hidden-input {
  display: none;
}

/* ✅ 아이콘 스타일 */
.toggle-icon {
  width: 20px;
  height: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* ✅ 비활성화 상태 */
.base-toggle.disabled {
  cursor: not-allowed;
}
</style>
