<template>
  <div class="filter-container">
    <div class="filter-header">
      <h4>검색 필터</h4>
      <!-- 🔹 버튼 그룹이 비어 있어도 div는 유지 -->
      <div class="button-group">
        <BaseButton
          v-for="(button, index) in filterButtons"
          :key="index"
          :type="button.type"
          :class="button.class"
          @click="handleClick(button)"
        >
          {{ button.label }}
        </BaseButton>
      </div>
    </div>

    <div class="filter-box">
      <!-- 🔹 날짜 범위 입력 -->
      <BaseDateRange
        label="최종 접속일"
        :startDate="startDate"
        :endDate="endDate"
        @update:startDate="startDate = $event"
        @update:endDate="endDate = $event"
      />

      <!-- 🔹 filters가 존재하지 않아도 기본 구조 유지 -->
      <BaseInput
        v-for="(filter, index) in filters"
        :key="index"
        v-model="filter.model"
        :type="filter.type"
        :label="filter.label"
        :options="filter.options"
        :placeholder="filter.placeholder"
      />
    </div>
  </div>
</template>

<script>
import BaseButton from "./BaseButton.vue";
import BaseInput from "./BaseInput.vue";
import BaseDateRange from "./BaseDateRange.vue"; // 날짜 범위 입력 추가

export default {
  name: "FilterBox",
  components: { BaseButton, BaseInput, BaseDateRange },
  data() {
    return {
      startDate: "",
      endDate: "",
    };
  },
  props: {
    filters: {
      type: Array,
      default: () => [], // 🔹 기본값을 빈 배열로 설정
    },
    filterButtons: {
      type: Array,
      default: () => [], // 🔹 기본값을 빈 배열로 설정
    },
  },
  methods: {
    handleClick(button) {
      console.log(`${button.label} 버튼 클릭됨!`);
      this.$emit("filter-action", button);
    },
  },
};
</script>

<style scoped>
.filter-container {
  margin: 20px 35px 30px 35px;
}

.filter-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 13px;
  padding-right: 10px;
}

/* 버튼 간격 조정 */
.button-group {
  display: flex;
  gap: 10px;
  min-height: 32px; /* 🔹 버튼이 없을 때도 레이아웃 유지 */
}

/* 검색 필터 타이틀 스타일 */
.filter-header h4 {
  position: relative;
  font-size: var(--font-size-base);
  color: var(--primary-color);
  padding-left: 21px;
}

.filter-header h4::before {
  content: "";
  position: absolute;
  left: 10px;
  top: 50%;
  width: 6px;
  height: 6px;
  background-color: var(--primary-color);
  transform: translateY(-50%);
}

/* 검색 필터 박스 */
.filter-box {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 15px;
  background-color: var(--LightGray-color);
  border: 1px solid var(--lineStroke-color);
  padding: 20px 30px;
  width: 100%;
  min-height: 50px; /* 🔹 필터 박스가 사라지지 않도록 최소 높이 설정 */
}
</style>
