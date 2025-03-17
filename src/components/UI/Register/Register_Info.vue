<template>
  <div class="info-header">
    <div class="header-title">
      <h4>{{ title }}</h4>
      <div class="button-group icon-search">
        <!-- ✅ 아이콘이 있을 때만 표시 -->

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
    <!-- ✅ AdminSearchPopup 팝업 추가 -->
    <AdminSearchPopup v-if="isPopupOpen" @close="closePopup" />
    <div class="body-cont">
      <dl>
        <dt>회사</dt>
        <dd>-</dd>
        <dt>이름</dt>
        <dd>-</dd>
        <dt>IKEN ID</dt>
        <dd>-</dd>
        <dt>사용 여부</dt>
        <dd>
          <BaseRadio
            v-model="usageStatus"
            :options="[
              { label: '사용', value: 'active' },
              { label: '미사용', value: 'inactive' },
            ]"
          />
        </dd>
        <dt>로그인 2차 인증 여부</dt>
        <dd>
          <BaseRadio
            v-model="authStatus"
            :options="[
              { label: '인증', value: 'verified' },
              { label: '미인증', value: 'unverified' },
            ]"
          />
        </dd>
        <dt>비고</dt>
        <dd class="Textarea-dd">
          <BaseTextarea
            v-model="remark"
            placeholder="비고를 입력하세요..."
            :maxLength="1000"
            rows="3"
          />
        </dd>
      </dl>
    </div>
  </div>
</template>

<script setup>
import { defineProps, defineEmits, ref } from "vue";
import BaseButton from "../BaseButton.vue";
import BaseTextarea from "../BaseTextarea.vue";
import BaseRadio from "../BaseRadio.vue";

const props = defineProps({
  title: { type: String, required: true }, // 🔹 제목을 props로 받음
  buttons: {
    // 🔹 버튼 목록을 props로 받음
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["button-click"]);
const remark = ref("");
const usageStatus = ref("active"); // ✅ "사용 여부" 선택값
const authStatus = ref("unverified"); // ✅ "로그인 2차 인증 여부" 선택값

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
  padding: 9px 10px 9px 10px;
  color: var(--fontbk-color);
}
.body-cont dl dd.Textarea-dd {
  margin-bottom: 0px;
  padding: 0;
}
.icon-search button {
  background: var(--white-color) url(@/images/icon_search.png) no-repeat left
    10px center;
  padding-left: 33px;
}
</style>
