<template>
  <div class="page-container">
    <!-- 상단 헤더 컴포넌트 -->
    <AdminHeader
      title="관리자 등록/관리"
      :buttons="buttons"
      @button-click="handleClick"
    />

    <!-- 검색 필터 컴포넌트 -->
    <FilterBox
      title="관리자 검색"
      :filters="filters"
      :filterButtons="filterButtons"
      @filter-search="searchFilters"
      @filter-reset="resetFilters"
    />

    <div class="content-area">
      <div class="table-main">
        <!-- 테이블 헤더 -->
        <div class="table-header">
          <h5>시스템관리자 100명</h5>
          <div class="button-group">
            <BaseButton
              v-for="button in tableHeaderButtons"
              :key="button.label"
              :label="button.label"
              :type="button.type"
              :size="button.size"
              @click="handleTableHeaderClick(button)"
            />
          </div>
        </div>

        <!-- 테이블 내용 -->
        <div class="table-content">
          <table>
            <thead>
              <tr>
                <th>
                  <input
                    type="checkbox"
                    v-model="allChecked"
                    @change="toggleAll"
                  />
                </th>
                <th>사용자 ID</th>
                <th>사용자 이름</th>
                <th>최종 접속일</th>
                <th>사용 여부</th>
                <th>최종 수정자 ID</th>
                <th>최종 수정자 이름</th>
                <th>최종 수정 일시</th>
                <th>비고</th>
                <th></th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(user, index) in filteredUsers"
                :key="index"
                :class="{
                  'selected-row': selectedUsers.includes(user.id),
                  'inactive-row': user.status === '미사용', // ✅ 미사용이면 회색 처리
                }"
              >
                <td>
                  <input
                    type="checkbox"
                    v-model="selectedUsers"
                    :value="user.id"
                  />
                </td>
                <td>{{ user.id }}</td>
                <td>{{ user.name }}</td>
                <td>{{ user.lastLogin }}</td>
                <td :class="{ 'inactive-text': user.status === '미사용' }">
                  {{ user.status }}
                </td>
                <td>{{ user.editorId }}</td>
                <td>{{ user.editorName }}</td>
                <td>{{ user.editDate }}</td>
                <td>-</td>
                <td>
                  <div class="button-group">
                    <BaseButton
                      label="상세"
                      type="table-content-default"
                      size="small"
                      @click="goToDetail(user.id)"
                    />
                    <BaseButton
                      label="삭제"
                      type="table-content-danger"
                      size="small"
                      @click="deleteRow(user.id)"
                    />
                  </div>
                </td>
              </tr>
            </tbody>
          </table>
        </div>

        <!-- 페이지네이션 & 행 개수 선택 -->
        <div class="table-bottom">
          <div class="pagination">
            <button @click="prevPage" :disabled="currentPage === 1">
              &lt;
            </button>
            <button
              v-for="page in totalPages"
              :key="page"
              @click="goToPage(page)"
              :class="{ active: page === currentPage }"
            >
              {{ page }}
            </button>
            <button @click="nextPage" :disabled="currentPage === totalPages">
              &gt;
            </button>
          </div>

          <div class="row-selector">
            <label>행 개수:</label>
            <select v-model="rowsPerPage">
              <option value="10">10개씩 보기</option>
              <option value="20">20개씩 보기</option>
              <option value="30">30개씩 보기</option>
            </select>
          </div>
        </div>
      </div>

      <!-- 📌 ContentArea 내부에서 동적으로 page_Admin.vue를 로드 -->
      <ContentArea
        :folderName="'components/pages/SysMgmt'"
        :componentName="'page_Admin'"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";
import AdminHeader from "@/components/UI/AdminHeader.vue";
import FilterBox from "@/components/UI/FilterBox.vue";
import BaseButton from "@/components/UI/BaseButton.vue";

const buttons = ref([
  { label: "추가", type: "primary", size: "small" },
  { label: "삭제", type: "danger" },
]);

const filterButtons = ref([
  { label: "초기화", type: "secondary", size: "small" },
  { label: "검색", type: "primary", size: "small" },
]);

const filters = ref([
  {
    model: { startDate: "", endDate: "" },
    type: "date-range",
    label: "최종 접속일",
  },
  {
    model: "all",
    type: "select",
    label: "사용 여부",
    options: [
      { label: "전체", value: "all" },
      { label: "사용", value: "use" },
      { label: "미사용", value: "not-use" },
    ],
  },
  {
    model: "",
    type: "text",
    label: "ID",
    placeholder: "사용자 혹은 등록자 이름 입력",
  },
  {
    model: "",
    type: "text",
    label: "이름",
    placeholder: "사용자 혹은 등록자 이름 입력",
  },
]);

const users = ref(
  Array.from({ length: 28 }, (_, i) => ({
    id: `user${i + 1}`,
    name: `사용자${i + 1}`,
    lastLogin: `2025.01.${(i % 30) + 1} 15:30`,
    status: i % 2 === 0 ? "사용" : "미사용",
    editorId: `editor${i + 1}`,
    editorName: `수정자${i + 1}`,
    editDate: `2025.01.${(i % 30) + 1} 15:30`,
  }))
);

const selectedUsers = ref([]);
const currentPage = ref(1);
const rowsPerPage = ref(10);
const tableHeaderButtons = ref([
  { label: "선택 삭제", type: "danger", size: "small" },
  { label: "등록", type: "default", size: "small" },
]);

const selectedStatus = computed({
  get: () =>
    filters.value.find((filter) => filter.label === "사용 여부")?.model ||
    "all",
  set: (value) => {
    const statusFilter = filters.value.find(
      (filter) => filter.label === "사용 여부"
    );
    if (statusFilter) statusFilter.model = value;
  },
});

const allChecked = computed(() => {
  return (
    selectedUsers.value.length === paginatedUsers.value.length &&
    selectedUsers.value.length > 0
  );
});

const toggleAll = () => {
  if (allChecked.value) {
    selectedUsers.value = [];
  } else {
    selectedUsers.value = paginatedUsers.value.map((user) => user.id);
  }
};

const filteredUsers = computed(() => {
  if (!users.value) return [];
  if (selectedStatus.value === "all") return users.value;
  return users.value.filter((user) =>
    selectedStatus.value === "use"
      ? user.status === "사용"
      : user.status === "미사용"
  );
});

const totalPages = computed(() => {
  const perPage = Number(rowsPerPage.value) || 10;
  return Math.ceil(filteredUsers.value.length / perPage);
});

const paginatedUsers = computed(() => {
  const perPage = Number(rowsPerPage.value) || 10;
  const startIndex = (currentPage.value - 1) * perPage;
  return filteredUsers.value.slice(startIndex, startIndex + perPage);
});

watch(rowsPerPage, (newVal) => {
  rowsPerPage.value = Number(newVal) || 10;
  currentPage.value = 1;
});

watch([rowsPerPage, filteredUsers], () => {
  currentPage.value = 1;
});

watch(users, () => {
  currentPage.value = 1;
  rowsPerPage.value = 10; // 초기 로드시 10개만 보이게 설정
});

const goToPage = (page) => {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page;
  }
};

const prevPage = () => {
  if (currentPage.value > 1) currentPage.value--;
};

const nextPage = () => {
  if (currentPage.value < totalPages.value) currentPage.value++;
};
</script>

<style scoped>
.content-area {
  padding: 0px 35px 35px 35px;
}

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 20px 8.5px;
}

.table-header h5 {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
}

.vertical-bar {
  width: 1px;
  height: 24px;
  background-color: var(--lineStroke-color);
}

/* 테이블 및 페이지네이션 스타일 */
table {
  width: 100%;
  border-collapse: collapse;
}

th {
  background-color: var(--LightGray-color);
  padding: 12px 10px 13px 10px;
  text-align: left;
  border-bottom: 1px solid var(--lineStroke-color);
  border-top: 1px solid var(--lineStroke-color);
  font-size: var(--font-size-base);
}

td {
  padding: 12px 10px 13px 10px;
  text-align: left;
  border-bottom: 1px solid var(--lineStroke-color);
  font-size: var(--font-size-base);
}

.pagination button {
  margin: 5px;
}

.row-selector {
  margin-top: 10px;
}

.table-buttons {
  display: flex;
  justify-content: center; /* 버튼 중앙 정렬 */
  align-items: center;
  gap: 5px; /* 버튼 사이 간격 */
  flex-wrap: nowrap; /* 버튼이 한 줄에 유지되도록 설정 */
}

/* 페이지네이션 & 행 개수 선택을 가로 정렬 */
.pagination-container {
  display: flex;
  justify-content: space-between; /* 좌우 정렬 */
  align-items: center;
  padding: 10px 30px;
  border-bottom: 1px solid var(--lineStroke-color);
}

/* 페이지네이션 버튼 스타일 */
.pagination {
  display: flex;
  gap: 5px;
}

.pagination button {
  padding: 5px 10px;
  stroke: var(--fontbk-color);
  color: var(--GrayHintText-color);
  cursor: pointer;
}

.pagination button.active {
  color: var(--fontbk-color);
}

.pagination button:disabled {
  cursor: not-allowed;
  opacity: 0.3;
}

/* 행 개수 선택 드롭다운 스타일 */
.row-selector {
  display: flex;
  align-items: center;
  gap: 10px;
}

.row-selector label {
  font-size: 14px;
  font-weight: bold;
}

.row-selector select {
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.table-main {
  padding: 0px 35px 35px 35px;
}

.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 8.5px;
}

.table-content table {
  width: 100%;
  border-collapse: collapse;
}

th {
  background-color: var(--LightGray-color);
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid var(--lineStroke-color);
}

td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid var(--lineStroke-color);
}

.button-group {
  display: flex;
  gap: 5px;
}

.selected-row {
  background-color: lightgreen;
}

.inactive-row td {
  color: #777777;
}

.table-bottom {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 10px;
}

.pagination button {
  margin: 0 5px;
}

.pagination .active {
  font-weight: bold;
}

.row-selector select {
  padding: 5px;
}
</style>
