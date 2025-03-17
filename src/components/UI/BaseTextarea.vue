<template>
  <div class="textarea-container">
    <label v-if="label">{{ label }}</label>
    <textarea
      :value="modelValue"
      :placeholder="placeholder"
      :rows="rows"
      :cols="cols"
      :maxlength="maxLength"
      class="custom-textarea"
      @input="updateValue"
    ></textarea>

    <!-- ✅ (현재 글자 수 / 최대 글자 수) 표시 -->
    <div class="char-count">
      <span class="current-count">{{ formattedCurrentLength }}</span> /
      <span class="max-count">{{ formattedMaxLength }}</span>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, computed } from "vue";

// Props 정의
const props = defineProps({
  modelValue: String,
  placeholder: { type: String, default: "내용을 입력하세요..." },
  label: { type: String, default: "" },
  rows: { type: Number, default: 5 },
  cols: { type: Number, default: 50 },
  maxLength: { type: Number, default: 1000 }, // ✅ 최대 글자 수 제한 추가
});

// v-model 동작을 위해 emit 정의
const emit = defineEmits(["update:modelValue"]);

// ✅ 현재 입력한 글자 수 계산
const currentLength = computed(() => props.modelValue?.length || 0);

// ✅ 숫자에 천 단위(,) 추가하는 함수
const formatNumber = (num) => num.toLocaleString();

// ✅ 현재 글자 수와 최대 글자 수를 변환된 형식으로 표시
const formattedCurrentLength = computed(() =>
  formatNumber(currentLength.value)
);
const formattedMaxLength = computed(() => formatNumber(props.maxLength));

const updateValue = (event) => {
  emit("update:modelValue", event.target.value);
};
</script>

<style scoped>
.textarea-container {
  position: relative;
  width: 100%;
  resize: none;
  padding: 10px 10px 41px 10px;
  border: 1px solid var(--lineStroke-color);
  box-sizing: border-box;
  border-radius: var(--spacing-xs);
}

label {
  font-weight: bold;
}

.custom-textarea {
  width: 100%;
  height: 91px;
  border: none;
  border-radius: var(--spacing-xs);
  font-size: var(--font-size-base);
  resize: none;
}
.custom-textarea:focus {
  outline: 0px solid var(--lineStroke-color);
}

/* ✅ 글자 수 카운트 스타일 */
.char-count {
  position: absolute;
  right: 10px;
  bottom: 10px;
  font-size: var(--font-size-base);
}

/* ✅ 현재 입력한 글자 수 스타일 */
.current-count {
  color: var(--fontbk-color);
}

/* ✅ 최대 글자 수 스타일 */
.max-count {
  color: var(--Gray-color);
}
</style>
