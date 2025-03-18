<template>
  <div class="popup-wrapper">
    <div class="popup-layer">
      <!-- ✅ 팝업 헤더 (유지) -->
      <div class="popup-header">
        <h3>관리자 검색</h3>
        <button class="close-btn" @click="closePopup">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            fill="none"
          >
            <path
              fill-rule="evenodd"
              clip-rule="evenodd"
              d="M21.722 21.722a1 1 0 0 0 0-1.414L16.344 15l5.378-5.379a1 1 0 0 0-1.414-1.414L15 13.586 9.621 8.207a1 1 0 1 0-1.414 1.414L13.586 15l-5.379 5.378a1 1 0 0 0 1.414 1.414L15 16.414l5.378 5.308a1 1 0 0 0 1.344 0z"
              fill="#1C1C1C"
            />
          </svg>
        </button>
      </div>

      <!-- ✅ 팝업 바디 -->
      <div class="popup-body">
        <!-- 🔹 검색 필터 -->
        <div class="search-filters">
          <label>회사:</label>
          <BaseInput
            v-model="filters.company"
            type="select"
            :options="companyOptions"
          />

          <label>이름:</label>
          <BaseInput
            v-model="filters.name"
            type="text"
            placeholder="이름 입력"
          />

          <label>IKEN ID:</label>
          <BaseInput
            v-model="filters.ikenId"
            type="text"
            placeholder="IKEN ID 입력"
          />

          <BaseButton label="검색" @click="searchData" />
        </div>

        <!-- 🔹 결과 테이블 -->
        <TableContent :headers="tableHeaders" :data="tableData">
          <template v-slot:row="{ item }">
            <td>
              <input
                type="radio"
                name="selectedUser"
                v-model="selectedUser"
                :value="item.ikenId"
              />
            </td>
            <td>{{ item.company }}</td>
            <td>{{ item.department }}</td>
            <td>{{ item.name }}</td>
            <td>{{ item.ikenId }}</td>
          </template>
        </TableContent>
      </div>

      <!-- ✅ 팝업 푸터 (유지) -->
      <div class="popup-footer">
        <BaseButton label="취소" @click="closePopup" />
        <BaseButton label="확인" type="primary" @click="confirmSelection" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, defineEmits } from "vue";
import BaseInput from "../BaseInput.vue";
import BaseButton from "../BaseButton.vue";
import TableContent from "../Table/TableContent.vue";

const emit = defineEmits(["close"]);

// ✅ 검색 필터 데이터
const filters = ref({
  company: "",
  name: "",
  ikenId: "",
});

// ✅ 회사 선택 옵션
const companyOptions = ref([
  { label: "코오롱베니트", value: "kolon_benit" },
  { label: "코오롱인더스트리", value: "kolon_industry" },
]);

// ✅ 테이블 헤더
const tableHeaders = ref(["", "회사", "부서", "이름", "IKEN ID"]);

// ✅ 테이블 데이터 (예제 데이터)
const tableData = ref([
  {
    company: "코오롱베니트",
    department: "Service Design팀",
    name: "홍길동",
    ikenId: "gildong_hong",
  },
  {
    company: "코오롱베니트",
    department: "IT팀",
    name: "김철수",
    ikenId: "chulsoo_kim",
  },
]);

// ✅ 선택된 사용자
const selectedUser = ref(null);

// ✅ 검색 기능 (필터링 예제)
const searchData = () => {
  console.log("검색 실행:", filters.value);
};

// ✅ 확인 버튼 클릭 시 처리
const confirmSelection = () => {
  console.log("선택된 사용자:", selectedUser.value);
  closePopup();
};

// ✅ 팝업 닫기
const closePopup = () => {
  emit("close");
};
</script>

<style scoped>
/* ✅ 팝업 스타일 */
.popup-wrapper {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgba(0, 0, 0, 0.3);
  z-index: 9999;
}

.popup-layer {
  background: white;
  padding: 20px;
  width: 600px;
  border-radius: 8px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
}

/* ✅ 헤더 스타일 */
.popup-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 18px;
  font-weight: bold;
  padding-bottom: 10px;
}

/* ✅ 검색 필터 스타일 */
.search-filters {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 15px;
}

.search-filters label {
  font-size: 14px;
  font-weight: bold;
}

/* ✅ 푸터 스타일 */
.popup-footer {
  display: flex;
  justify-content: flex-end;
  padding-top: 15px;
  gap: 10px;
}
</style>
