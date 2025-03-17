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
      <dl>
        <dt>최종 수정자 이름</dt>
        <dd>배유진</dd>
        <dt>최종 수정자 IKEN ID</dt>
        <dd>yujin_bae</dd>
        <dt>최종 수정 일시</dt>
        <dd>-</dd>
      </dl>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import BaseButton from "../BaseButton.vue";

const props = defineProps({
  title: { type: String, required: true },
  buttons: { type: Array, default: () => [] },
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
  padding: 20px 37px 21px 30px;
  border-radius: var(--spacing-xs);
  border: 1px solid var(--lineStroke-color);
}
.body-cont dl dt {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
  font-weight: bold;
  margin-bottom: 9px;
}
.body-cont dl dd {
  margin-bottom: 10px;
}
.body-cont dl dd {
  margin-bottom: 10px;
  padding: 9px 10px 9px 10px;
}
.body-cont dl dd:last-child {
  margin-bottom: 0px;
}
</style>
