<template>
  <div
    class="input-container"
    :class="{ error: errorMessage, 'auto-filled': autoFilled }"
  >
    <label v-if="label">{{ label }}</label>

    <div class="input-wrapper">
      <input
        v-if="type !== 'select'"
        :type="type"
        :placeholder="placeholder"
        :value="modelValue"
        :disabled="disabled"
        :readonly="readonly || autoFilled"
        @input="$emit('update:modelValue', $event.target.value)"
        @focus="isFocused = true"
        @blur="handleBlur"
      />

      <!-- 삭제 버튼 (셀렉트 박스에서는 보이지 않음) -->
      <button v-if="showClearButton" class="clear-btn" @click="clearInput">
        ×
      </button>

      <!-- 날짜 입력일 경우, 커스텀 아이콘 추가 -->
      <span v-if="type === 'date'" class="calendar-icon" @click="openCalendar">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="17"
          viewBox="0 0 16 17"
          fill="none"
        >
          <rect
            x="0.5"
            y="0.75"
            width="15"
            height="15"
            rx="1.5"
            fill="white"
            stroke="#1C1C1C"
          />
          <path
            d="M0 2.25C0 1.14543 0.895431 0.25 2 0.25H14C15.1046 0.25 16 1.14543 16 2.25V5.25H0V2.25Z"
            fill="#1C1C1C"
          />
          <rect x="5.7998" y="6.5498" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect x="8.7998" y="6.5498" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect
            x="11.7998"
            y="6.5498"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
        </svg>
      </span>

      <!-- 셀렉트 박스 -->
      <select
        v-if="type === 'select'"
        :value="modelValue"
        :disabled="disabled"
        @change="$emit('update:modelValue', $event.target.value)"
      >
        <option
          v-for="(option, index) in options"
          :key="index"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
    </div>

    <!-- 에러 메시지 -->
    <p v-if="errorMessage" class="error-text">{{ errorMessage }}</p>
  </div>
</template>

<script>
export default {
  name: "BaseInput",
  props: {
    modelValue: [String, Number],
    label: String,
    placeholder: String,
    type: { type: String, default: "text" },
    errorMessage: String,
    disabled: Boolean,
    readonly: Boolean,
    autoFilled: Boolean,
    options: { type: Array, default: () => [] },
  },
  data() {
    return {
      isFocused: false,
    };
  },
  computed: {
    showClearButton() {
      return (
        this.modelValue &&
        !this.disabled &&
        !this.readonly &&
        !this.autoFilled &&
        !this.isFocused &&
        this.type !== "select" // 셀렉트 박스에서는 삭제 버튼 숨김
      );
    },
  },
  methods: {
    handleBlur() {
      this.isFocused = false;
    },
    clearInput() {
      this.$emit("update:modelValue", "");
    },
    openCalendar() {
      this.$el.querySelector("input[type='date']").showPicker();
    },
  },
};
</script>

<style scoped>
.input-container {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

input {
  width: 100%;
  padding: 5px 10px 6px 10px;
  border-radius: var(--spacing-xs);
  border: 1px solid var(--lineStroke-color);
  font-size: var(--font-size-base);
  box-sizing: border-box;
}
select {
  width: 100%;
  padding: 7px 10px 6px 10px;
  border-radius: var(--spacing-xs);
  border: 1px solid var(--lineStroke-color);
  font-size: var(--font-size-base);
  box-sizing: border-box;
}

input:focus,
select:focus {
  border-color: var(--primary-color);
  outline: none;
}

input[disabled],
select[disabled] {
  background: var(--LightGray-color);
  cursor: not-allowed;
}

/* 자동 완성된 상태 */
.auto-filled input {
  background-color: var(--white-color);
  border-color: var(--white-color);
  cursor: default;
  color: var(--fontbk-color);
}

/* 삭제 버튼 */
.clear-btn {
  position: absolute;
  right: 10px;
  background: none;
  border: none;
  font-size: var(--font-size-base);
  cursor: pointer;
}

/* 에러 메시지 */
.error-text {
  color: #fc5a5a;
}

/* 날짜 입력 커스텀 */
.calendar-icon {
  position: absolute;
  top: 10px;
  right: 12px;
  cursor: pointer;
}
</style>
