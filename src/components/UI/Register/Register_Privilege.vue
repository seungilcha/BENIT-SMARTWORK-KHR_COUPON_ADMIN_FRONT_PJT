<template>
  <div class="info-header">
    <div class="header-title">
      <h4>{{ title }}</h4>
      <div class="button-group">
        <BaseButton
          v-for="(button, index) in buttons"
          :key="index"
          :label="button.label"
          :type="button.type"
          :size="button.size"
          @click="handleClick(button)"
        />
      </div>
    </div>
    <div class="body-cont">
      <TreeCheckbox />
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import BaseButton from "../BaseButton.vue";
import TreeCheckbox from "../BaseTree.vue";

const props = defineProps({
  title: { type: String, required: true }, // 🔹 제목을 props로 받음
  buttons: {
    // 🔹 버튼 목록을 props로 받음
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["button-click"]);

const handleClick = (button) => {
  emit("button-click", button);
};
</script>

<style scoped>
.header-title {
  display: flex;
  align-items: center;
  gap: 11px;
  line-height: 35px;
}

.info-header h4 {
  position: relative;
  font-size: var(--font-size-base);
  color: var(--primary-color);
  padding-left: 21px;
}

.info-header h4::before {
  content: "";
  position: absolute;
  left: 10px;
  top: 50%;
  width: 6px;
  height: 6px;
  background-color: var(--primary-color);
  transform: translateY(-50%);
}
.body-cont {
  margin-top: 10px;
  background-color: var(--white-color);
  border-radius: var(--spacing-xs);
  border: 1px solid var(--lineStroke-color);
}
</style>
