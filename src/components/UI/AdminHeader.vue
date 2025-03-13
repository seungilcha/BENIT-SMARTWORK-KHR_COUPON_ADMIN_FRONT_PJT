<template>
  <div class="sub-header">
    <h1>{{ title }}</h1>
    <div v-if="buttons.length > 0" class="button-group">
      <BaseButton
        v-for="(button, index) in buttons"
        :key="index"
        :type="button.type"
        :class="button.class"
        @click="handleClick(button)"
      >
        {{ button.label }}
      </BaseButton>
    </div>
  </div>
</template>

<script>
import BaseButton from "./BaseButton.vue";

export default {
  name: "AdminHeader",
  components: { BaseButton },
  props: {
    title: String, // 헤더 제목
    buttons: Array, // 버튼 리스트
  },
  methods: {
    handleClick(button) {
      console.log(`${button.label} 버튼 클릭됨!`);
      this.$emit("button-click", button);
    },
  },
};
</script>

<style scoped>
.sub-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 18px 35px;
  background-color: var(--white-color);
  border-bottom: 1px solid var(--lineStroke-color);
}

.sub-header h1 {
  font-size: var(--font-size-xl);
  color: var(--fontbk-color);
  line-height: 46px;
}

.button-group {
  display: flex;
  gap: 10px;
  margin-right: 10px;
}
</style>
