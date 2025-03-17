<template>
  <div
    class="input-container"
    :class="{ error: errorMessage, 'auto-filled': autoFilled }"
  >
    <label v-if="label">{{ label }}</label>

    <!-- 📌 일반 날짜 입력 -->
    <div v-if="type === 'date'" class="input-wrapper" @click="openDateCalendar">
      <input
        type="text"
        :value="formattedDate"
        readonly
        placeholder="YYYY.MM.DD"
      />
      <input
        type="date"
        ref="dateInput"
        v-model="dateValue"
        @input="updateDate($event)"
        class="hidden-input"
      />
      <span class="calendar-icon" @click="openDateCalendar">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="17"
          viewBox="0 0 16 17"
          fill="none"
        >
          <rect
            x="0.5"
            y="1"
            width="15"
            height="15"
            rx="1.5"
            fill="white"
            stroke="#1C1C1C"
          />
          <path
            d="M0 2.5C0 1.39543 0.895431 0.5 2 0.5H14C15.1046 0.5 16 1.39543 16 2.5V5.5H0V2.5Z"
            fill="#1C1C1C"
          />
          <rect x="5.7998" y="6.7998" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect x="5.7998" y="9.7998" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect
            x="5.7998"
            y="12.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
          <rect x="2.7998" y="9.7998" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect
            x="2.7998"
            y="12.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
          <rect x="8.7998" y="6.7998" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect x="8.7998" y="9.7998" width="1.5" height="1.5" fill="#1C1C1C" />
          <rect
            x="8.7998"
            y="12.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
          <rect
            x="11.7998"
            y="6.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
          <rect
            x="11.7998"
            y="9.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
          <rect
            x="11.7998"
            y="12.7998"
            width="1.5"
            height="1.5"
            fill="#1C1C1C"
          />
        </svg>
      </span>
    </div>

    <!-- 📌 날짜 범위 입력 -->
    <div v-else-if="type === 'date-range'" class="date-range-wrapper">
      <div class="input-wrapper" @click="openStartCalendar">
        <input
          type="text"
          :value="formattedStartDate"
          readonly
          placeholder="YYYY.MM.DD"
        />
        <input
          type="date"
          ref="startInput"
          v-model="startDate"
          @input="updateStartDate($event)"
          class="hidden-input"
        />
        <span class="calendar-icon" @click="openStartCalendar">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="17"
            viewBox="0 0 16 17"
            fill="none"
          >
            <rect
              x="0.5"
              y="1"
              width="15"
              height="15"
              rx="1.5"
              fill="white"
              stroke="#1C1C1C"
            />
            <path
              d="M0 2.5C0 1.39543 0.895431 0.5 2 0.5H14C15.1046 0.5 16 1.39543 16 2.5V5.5H0V2.5Z"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="2.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="2.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
          </svg>
        </span>
      </div>
      <span class="range-separator">~</span>
      <div class="input-wrapper" @click="openEndCalendar">
        <input
          type="text"
          :value="formattedEndDate"
          readonly
          placeholder="YYYY.MM.DD"
        />
        <input
          type="date"
          ref="endInput"
          v-model="endDate"
          @input="updateEndDate($event)"
          class="hidden-input"
        />
        <span class="calendar-icon" @click="openEndCalendar">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="17"
            viewBox="0 0 16 17"
            fill="none"
          >
            <rect
              x="0.5"
              y="1"
              width="15"
              height="15"
              rx="1.5"
              fill="white"
              stroke="#1C1C1C"
            />
            <path
              d="M0 2.5C0 1.39543 0.895431 0.5 2 0.5H14C15.1046 0.5 16 1.39543 16 2.5V5.5H0V2.5Z"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="5.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="2.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="2.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="8.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="6.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="9.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
            <rect
              x="11.7998"
              y="12.7998"
              width="1.5"
              height="1.5"
              fill="#1C1C1C"
            />
          </svg>
        </span>
      </div>
    </div>

    <!-- 일반 입력 필드 -->
    <div v-if="type !== 'select'" class="input-wrapper">
      <input
        v-if="computedShouldRenderInput"
        :type="type"
        :placeholder="placeholder"
        :value="computedModelValue"
        :disabled="disabled"
        :readonly="readonly || autoFilled"
        @input="emit('update:modelValue', $event.target.value)"
        @focus="isFocused = true"
        @blur="handleBlur"
      />
      <button v-if="showClearButton" class="clear-btn" @click="clearInput">
        ×
      </button>
    </div>

    <!-- 📌 셀렉트 박스 -->
    <div
      v-else-if="type === 'select'"
      class="select-wrapper"
      @click="toggleSelect"
    >
      <select
        v-model="selectedValue"
        @change="handleSelectChange"
        @focus="isOpen = false"
        @blur="isOpen = true"
        :disabled="disabled"
      >
        <option
          v-for="option in options"
          :key="option.value"
          :value="option.value"
        >
          {{ option.label }}
        </option>
      </select>
      <span class="select-icon">
        <!-- 옵션이 펼쳐지면 화살표 위로 -->
        <svg
          v-if="isOpen"
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          viewBox="0 0 16 16"
          fill="none"
        >
          <path
            d="M4 10L8 6L12 10"
            stroke="black"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
        <!-- 옵션이 접히면 화살표 아래로 -->
        <svg
          v-else
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="16"
          viewBox="0 0 16 16"
          fill="none"
        >
          <path
            d="M4 6L8 10L12 6"
            stroke="black"
            stroke-width="1.5"
            stroke-linecap="round"
            stroke-linejoin="round"
          />
        </svg>
      </span>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watchEffect, defineProps, defineEmits } from "vue";
import dayjs from "dayjs";
const safeOptions = computed(() => props.options ?? []); // options가 undefined면 빈 배열 반환

const props = defineProps({
  label: String,
  type: { type: String, default: "text" },
  placeholder: String,
  modelValue: [String, Number, Object], // modelValue가 객체일 수도 있으므로 추가
  disabled: Boolean,
  readonly: Boolean,
  errorMessage: String,
  autoFilled: Boolean,
  options: { type: Array, default: [] }, // ✅ options 기본값 추가
  startDate: String,
  endDate: String,
});
const emit = defineEmits([
  "update:modelValue",
  "update:startDate",
  "update:endDate",
]);

const isOpen = ref(false);
const isFocused = ref(false);

const showClearButton = computed(
  () =>
    !!props.modelValue &&
    !props.disabled &&
    !props.readonly &&
    ["text", "password", "email", "number", "tel", "search"].includes(
      props.type
    )
);

// ✅ modelValue가 객체일 경우 빈 값("") 반환
const computedModelValue = computed(() => {
  if (typeof props.modelValue === "object" || props.modelValue == null) {
    return "";
  }
  return String(props.modelValue);
});

// ✅ modelValue가 유효하지 않으면 input 자체를 렌더링하지 않음
const computedShouldRenderInput = computed(() => {
  return !(typeof props.modelValue === "object" || props.modelValue == null);
});
const selectedValue = ref(
  props.modelValue || (props.options.length > 0 ? props.options[0].value : "")
);

watchEffect(() => {
  if (!props.modelValue && safeOptions.value.length > 0) {
    selectedValue.value = safeOptions.value[0].value;
    emit("update:modelValue", selectedValue.value);
  }
});
const clearInput = () => emit("update:modelValue", "");
const handleBlur = () => (isFocused.value = false);
const handleSelectChange = (event) => {
  selectedValue.value = event.target.value;
  emit("update:modelValue", selectedValue.value);
  isOpen.value = true; // ✅ 옵션 선택 후 화살표 아래로 변경
};
const toggleSelect = () => {
  isOpen.value = !isOpen.value; // ✅ 클릭할 때마다 열고 닫음
};
const startDate = ref(props.startDate || "");
const endDate = ref(props.endDate || "");
const dateValue = ref(props.modelValue || "");

const formattedDate = computed(() =>
  dateValue.value ? dayjs(dateValue.value).format("YYYY.MM.DD") : ""
);
const formattedStartDate = computed(() =>
  startDate.value ? dayjs(startDate.value).format("YYYY.MM.DD") : ""
);
const formattedEndDate = computed(() =>
  endDate.value ? dayjs(endDate.value).format("YYYY.MM.DD") : ""
);

const startInput = ref(null);
const endInput = ref(null);
const dateInput = ref(null);

const updateStartDate = (event) => {
  startDate.value = dayjs(event.target.value).format("YYYY-MM-DD");
  emit("update:startDate", startDate.value);
};

const updateEndDate = (event) => {
  endDate.value = dayjs(event.target.value).format("YYYY-MM-DD");
  emit("update:endDate", endDate.value);
};

const updateDate = (event) => {
  dateValue.value = dayjs(event.target.value).format("YYYY-MM-DD");
  emit("update:modelValue", dateValue.value);
};

const openStartCalendar = () => startInput.value?.showPicker();
const openEndCalendar = () => endInput.value?.showPicker();
const openDateCalendar = () => dateInput.value?.showPicker();
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

input[type="date"],
input[readonly][placeholder="YYYY.MM.DD"] {
  padding: 5px 35px 6px 10px !important;
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
  top: 8px;
  right: 12px;
  cursor: pointer;
}
/* 날짜 범위 입력을 한 줄로 정렬 */
.date-range-wrapper {
  display: flex;
  align-items: center;
  gap: 8px; /* 날짜 입력 필드 간 간격 */
  width: 100%;
}

/* 날짜 입력 필드 컨테이너 */
.date-range-wrapper .input-wrapper {
  flex: 1;
}

/* 날짜 구분자 (~) 스타일 */
.range-separator {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
  white-space: nowrap; /* 줄바꿈 방지 */
}
.hidden-input {
  position: absolute;
  width: 100%;
  height: 100%;
  opacity: 0;
  visibility: hidden;
  z-index: 1;
  padding: 5px 35px 6px 10px;
}
/* 📌 select 기본 화살표 제거 */
select {
  appearance: none;
  -webkit-appearance: none;
  -moz-appearance: none;
  background-color: white;
  border: 1px solid var(--lineStroke-color);
  padding: 7px 30px 6px 10px; /* ✅ 화살표 아이콘 공간 확보 */
  width: 100%;
  font-size: var(--font-size-base);
  border-radius: var(--spacing-xs);
}

/* 📌 select wrapper 스타일 */
.select-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}
/* 📌 select 화살표 아이콘 위치 */
.select-icon {
  position: absolute;
  right: 10px;
  pointer-events: none;
  display: flex;
  align-items: center;
}
</style>
