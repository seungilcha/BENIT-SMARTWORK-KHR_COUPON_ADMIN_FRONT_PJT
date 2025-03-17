<template>
  <div class="radio-group">
    <label v-for="option in options" :key="option.value" class="radio-label">
      <input
        type="radio"
        :value="option.value"
        :checked="modelValue === option.value"
        @change="updateValue(option.value)"
        class="hidden-radio"
      />
      <img
        :src="modelValue === option.value ? checkedIcon : uncheckedIcon"
        alt="radio button"
        class="radio-icon"
      />
      {{ option.label }}
    </label>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";

const props = defineProps({
  modelValue: String, // ✅ 선택된 값 (v-model 지원)
  options: {
    type: Array,
    required: true, // ✅ [{ label: '사용', value: 'active' }, { label: '미사용', value: 'inactive' }]
  },
  checkedIcon: {
    type: String,
    default: new URL("/src/images/icon-radio-on.svg", import.meta.url).href, // ✅ 강제 경로 설정
  },
  uncheckedIcon: {
    type: String,
    default: new URL("/src/images/icon-radio-off.svg", import.meta.url).href, // ✅ 강제 경로 설정
  },
});

const emit = defineEmits(["update:modelValue"]);

const updateValue = (value) => {
  emit("update:modelValue", value);
};
</script>

<style scoped>
.radio-group {
  display: flex;
  gap: 15px;
}

.radio-label {
  display: flex;
  align-items: center;
  gap: 5px;
  font-size: var(--font-size-base);
  color: var(--dark-bg-color);
  cursor: pointer;
}

/* ✅ 기본 라디오 버튼 숨기기 */
.hidden-radio {
  display: none;
}

/* ✅ 커스텀 라디오 버튼 스타일 */
.radio-icon {
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>
