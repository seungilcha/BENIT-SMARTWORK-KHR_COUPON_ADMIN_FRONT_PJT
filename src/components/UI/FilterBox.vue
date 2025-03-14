<template>
  <div class="filter-container">
    <div class="filter-header">
      <h4>{{ title }}</h4>
      <!-- 📌 동적으로 변경 가능하도록 title 사용 -->
      <div class="button-group">
        <BaseButton
          v-for="(button, index) in filterButtons"
          :key="index"
          :type="button.type"
          :size="button.size"
          :class="button.class"
          @click="handleClick(button)"
        >
          {{ button.label }}
        </BaseButton>
      </div>
    </div>

    <div class="filter-box">
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

<script setup>
import { toRefs, defineProps, defineEmits } from "vue";
import BaseButton from "./BaseButton.vue";
import BaseInput from "./BaseInput.vue";

const props = defineProps({
  title: { type: String, required: true },
  filterButtons: Array,
  filters: Array,
  startDate: String, // ✅ startDate를 props에서 받도록 수정
  endDate: String, // ✅ endDate를 props에서 받도록 수정
});
// toRefs() 사용하여 반응형 유지
const { filterButtons, filters } = toRefs(props);

const emit = defineEmits(["filter-click"]);

const handleClick = (button) => {
  emit("filter-click", button);
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
