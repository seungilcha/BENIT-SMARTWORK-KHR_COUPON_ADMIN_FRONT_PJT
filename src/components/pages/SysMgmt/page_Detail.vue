<template>
  <div class="page-container">
    <!-- 상단 헤더 컴포넌트 -->
    <AdminHeader title="관리자 등록/관리" :buttons="HeaderButtons" />
    <div class="page-container-group">
      <!-- 왼쪽 영역 -->
      <div class="group-left">
        <div class="register_info">
          <RegisterInfo
            title="관리자 검색"
            :buttons="registerInfoButtons"
            @button-click="onRegisterInfoButtonClick"
          />
          <AdminSearchPopup v-if="isPopupOpen" @close="closePopup" />
        </div>
        <div class="register_modifier">
          <RegisterModifier title="최종 수정자 정보" />
        </div>
        <!-- ✅ 버튼 추가 부분 -->
        <div class="button-wrapper">
          <BaseButton
            @click="handleNewButtonClick"
            label="목록"
            type="default"
            size="medium"
          /><!-- 버튼 활성화 type="primary" 변경 -->
        </div>
      </div>
      <!-- 오른쪽 영역 -->
      <div class="group-right">
        <div class="register_privilege">
          <RegisterPrivilege title="메뉴 권한 설정" />
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed, ref } from "vue";
import AdminHeader from "../../UI/AdminHeader.vue";
import BaseButton from "../../UI/BaseButton.vue";
import RegisterInfo from "../../UI/Register/Register_Info.vue";
import RegisterModifier from "../../UI/Register/Register_Modifier.vue";
import RegisterPrivilege from "../../UI/Register/Register_Privilege.vue";
import AdminSearchPopup from "../../UI/popup/AdminSearchPopup.vue";

// 헤더 버튼 정의
const HeaderButtons = computed(() => [
  {
    label: "등록",
    type: "danger",
    size: "large",
  },
]);

const registerInfoButtons = computed(() => [
  { label: "관리자 검색", type: "default", size: "small" },
]);

// ✅ "관리자 검색" 팝업 상태 관리
const isPopupOpen = ref(false);

// ✅ 버튼 클릭 시 실행
const onRegisterInfoButtonClick = (button) => {
  if (button.label === "관리자 검색") {
    isPopupOpen.value = true; // 팝업 열기
  }
};

// ✅ 팝업 닫기 이벤트
const closePopup = () => {
  isPopupOpen.value = false; // 팝업 닫기
};
</script>

<style scoped>
.page-container {
  display: flex;
  width: 100%;
  justify-content: space-between;
  flex-direction: column;
}
.page-container {
  display: flex;
  flex-direction: column;
}

.page-container-group {
  display: flex;
  flex: 1;
  gap: 30px; /* 좌우 간격 */
  padding: 20px 35px;
  background-color: #f3f5f6;
}

.group-left {
  flex: 2; /* 왼쪽 영역 */
  display: flex;
  flex-direction: column;
  gap: 10px; /* 내부 간격 */
}

.group-right {
  flex: 1; /* 오른쪽 영역 */
  display: flex;
  align-items: start;
  justify-content: left;
}
.header-title {
  display: flex;
  align-items: center;
  gap: 11px;
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
.register_privilege {
  width: 100%;
}

.body-cont {
  margin-top: 10px;
  background-color: var(--white-color);
  padding: 20px 37px 21px 30px;
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
</style>
