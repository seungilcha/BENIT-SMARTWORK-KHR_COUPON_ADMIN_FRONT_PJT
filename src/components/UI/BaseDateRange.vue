<template>
  <div class="date-range-container">
    <label v-if="label">{{ label }}</label>
    <div class="date-picker">
      <!-- 시작 날짜 -->
      <div class="input-wrapper">
        <input
          type="date"
          :value="startDate"
          @input="updateStartDate($event)"
          ref="startInput"
        />
        <span class="calendar-icon" @click="openStartCalendar"
          ><svg
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

      <!-- 종료 날짜 -->
      <div class="input-wrapper">
        <input
          type="date"
          :value="endDate"
          @input="updateEndDate($event)"
          ref="endInput"
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
  </div>
</template>

<script>
export default {
  name: "BaseDateRange",
  props: {
    label: String, // 라벨
    startDate: String, // 시작 날짜
    endDate: String, // 종료 날짜
  },
  methods: {
    updateStartDate(event) {
      this.$emit("update:startDate", event.target.value);
    },
    updateEndDate(event) {
      this.$emit("update:endDate", event.target.value);
    },
    openStartCalendar() {
      this.$refs.startInput.showPicker(); // 달력 열기
    },
    openEndCalendar() {
      this.$refs.endInput.showPicker(); // 달력 열기
    },
  },
};
</script>

<style scoped>
/* 날짜 범위 입력 컨테이너 */
.date-range-container {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

/* 날짜 입력 필드 */
.date-picker {
  display: flex;
  align-items: center;
  gap: 6px;
  width: 100%;
}

/* 날짜 입력 필드 래퍼 */
.input-wrapper {
  position: relative;
  flex: 1;
}

/* 기본 달력 아이콘 숨기기 */
input[type="date"] {
  width: 100%;
  padding: 5px 10px 6px 10px;
  border: 1px solid #ccc;
  border-radius: var(--spacing-xs);
  box-sizing: border-box;
  appearance: none; /* 기본 달력 아이콘 제거 */
  -webkit-appearance: none;
  -moz-appearance: none;
}

/* 커스텀 달력 아이콘 */
.calendar-icon {
  position: absolute;
  top: 50%;
  right: 10px;
  transform: translateY(-50%);
  cursor: pointer;
  font-size: 21px;
}

/* 날짜 범위 구분자 */
.range-separator {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
}
</style>
