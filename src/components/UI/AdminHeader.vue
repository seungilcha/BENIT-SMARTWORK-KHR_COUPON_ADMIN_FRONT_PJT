<template>
  <div class="sub-header">
    <h1>{{ props.title }}</h1>
    <div v-if="buttons.length > 0" class="button-group">
      <BaseButton
        v-for="(button, index) in buttons"
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
</template>

<script setup>
import { defineProps, defineEmits } from "vue";
import BaseButton from "./BaseButton.vue";

const props = defineProps({
  title: String,
  buttons: Array,
});

const emit = defineEmits(["button-click"]);

const handleClick = (button) => {
  emit("button-click", button);
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
  font-weight: bold;
}
</style>
