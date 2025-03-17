<template>
  <div class="table-header">
    <h5 v-if="title">
      {{ title }}
      <span v-if="totalUsers !== null"> {{ totalUsers }}{{ unit }}</span>
    </h5>

    <div class="button-group" v-if="tableHeaderButtons?.length">
      <template v-for="(button, index) in tableHeaderButtons" :key="index">
        <BaseButton
          :label="button.label"
          :type="button.type"
          :size="button.size"
          @click="
            button.action ? button.action() : console.log('⚠️ 버튼 액션 없음')
          "
        >
          <!-- ✅ 업로드 버튼: 기존 아이콘 유지 -->
          <template v-slot:icon v-if="button.icon">
            <component :is="button.icon" size="16" />
          </template>
        </BaseButton>
        <span v-if="button.showBar" class="vertical-bar"></span>
      </template>
    </div>
  </div>
</template>

<script setup>
import { defineEmits } from "vue";
import BaseButton from "@/components/UI/BaseButton.vue";

defineProps({
  title: { type: String, required: false, default: "" },
  tableHeaderButtons: { type: Array, required: false, default: () => [] },
  totalUsers: { type: Number, required: false, default: null },
  unit: { type: String, required: false, default: "명" },
});

const emit = defineEmits(["table-header-click"]);

const handleTableHeaderClick = () => {
  emit("table-header-click"); // ✅ 클릭 시 이벤트 발생
};
</script>

<style scoped>
.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 20px 8.5px 10px;
}
.table-header h5 {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
}
.vertical-bar {
  width: 1px;
  background-color: var(--lineStroke-color);
  margin: 5px 0px;
}
.download-icon {
  display: inline-block;
  transform: rotate(180deg);
}
</style>
