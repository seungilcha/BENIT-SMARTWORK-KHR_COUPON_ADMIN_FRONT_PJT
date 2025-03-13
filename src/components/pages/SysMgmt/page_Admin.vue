<template>
  <div class="admin-container">
    <!-- 상단 헤더 컴포넌트 -->
    <AdminHeader
      title="관리자 등록/관리"
      :buttons="buttons"
      @button-click="handleClick"
    />

    <!-- 검색 필터 컴포넌트 -->
    <FilterBox
      :filters="filters"
      :filterButtons="filterButtons"
      @filter-action="handleClick"
      @filter-reset="resetFilters"
      @filter-search="searchFilters"
    />

    <div class="content-area">
      <div class="content-header">
        <h5>콘텐츠 제목</h5>
        <div class="content-buttons">
          <template
            v-for="(button, index) in contentButtons"
            :key="button.label"
          >
            <BaseButton
              :label="button.label"
              :type="button.type"
              :size="button.size"
              @click="handleClick(button)"
            >
              <template v-slot:icon>
                <svg
                  v-if="button.label === '엑셀 업로드'"
                  xmlns="http://www.w3.org/2000/svg"
                  width="14"
                  height="14"
                  viewBox="0 0 14 14"
                  fill="none"
                >
                  <path
                    d="M1 13H13"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                  />
                  <path
                    d="M7 1L7 10"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                  />
                  <path
                    d="M2.5 4.6L7 1L11.5 4.6"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
                <svg
                  v-else-if="button.label === '엑셀 다운로드'"
                  xmlns="http://www.w3.org/2000/svg"
                  width="14"
                  height="14"
                  viewBox="0 0 14 14"
                  fill="none"
                >
                  <path
                    d="M1 1H13"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                  />
                  <path
                    d="M7 13L7 4"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                  />
                  <path
                    d="M2.5 9.4L7 13L11.5 9.4"
                    stroke="black"
                    stroke-width="1.5"
                    stroke-linecap="round"
                    stroke-linejoin="round"
                  />
                </svg>
              </template>
            </BaseButton>
            <span v-if="index === 1" class="vertical-bar"></span>
          </template>
        </div>
      </div>

      <!-- 📌 테이블 영역 -->
      <div class="table-container">
        <table>
          <thead>
            <tr>
              <th>
                <input
                  type="checkbox"
                  v-model="selectAll"
                  @change="toggleAllCheckboxes"
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
            <tr v-for="(row, index) in paginatedRows" :key="index">
              <td>
                <input type="checkbox" v-model="selectedRows" :value="row.id" />
              </td>
              <td>{{ row.userId }}</td>
              <td>{{ row.userName }}</td>
              <td>{{ row.lastLogin }}</td>
              <td>{{ row.status }}</td>
              <td>{{ row.modifierId }}</td>
              <td>{{ row.modifierName }}</td>
              <td>{{ row.modifiedAt }}</td>
              <td>{{ row.note }}</td>
              <td>
                <div class="table-buttons">
                  <BaseButton
                    label="상세"
                    type="table-content-default"
                    size="small"
                  />
                  <BaseButton
                    label="삭제"
                    type="table-content-default"
                    size="small"
                  />
                </div>
              </td>
            </tr>
          </tbody>
        </table>

        <!-- 📌 페이지네이션 + 행 개수 선택 -->
        <div class="pagination-container">
          <!-- 📌 왼쪽: 페이지네이션 -->
          <div class="pagination">
            <button @click="prevPage" :disabled="currentPage === 1">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="20"
                height="20"
                viewBox="0 0 20 20"
                fill="none"
              >
                <path
                  d="M11.2426 14.2426L7 10L11.2426 5.75736"
                  stroke-width="1.5"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
            <button
              v-for="page in totalPages"
              :key="page"
              @click="changePage(page)"
              :class="{ active: currentPage === page }"
            >
              {{ page }}
            </button>
            <button @click="nextPage" :disabled="currentPage === totalPages">
              <svg
                xmlns="http://www.w3.org/2000/svg"
                width="20"
                height="20"
                viewBox="0 0 20 20"
                fill="none"
              >
                <path
                  d="M8.75736 14.2426L13 10L8.75736 5.75736"
                  stroke-width="1.5"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                />
              </svg>
            </button>
          </div>

          <!-- 📌 오른쪽: 행 개수 선택 -->
          <div class="row-selector">
            <select v-model="rowsPerPage">
              <option
                v-for="option in [10, 20, 30]"
                :key="option"
                :value="option"
              >
                {{ option }}개씩 보기
              </option>
            </select>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AdminHeader from "@/components/UI/AdminHeader.vue";
import FilterBox from "@/components/UI/FilterBox.vue";
import BaseButton from "@/components/UI/BaseButton.vue";

export default {
  name: "SysMgmtAdmin",
  components: {
    AdminHeader,
    FilterBox,
    BaseButton,
  },
  data() {
    return {
      buttons: [{ label: "등록", type: "primary", size: "large" }],
      filterButtons: [
        {
          label: "초기화",
          type: "secondary",
          class: "btn-small",
          action: "reset",
        },
        {
          label: "검색",
          type: "primary",
          class: "btn-small",
          action: "search",
        },
      ],
      contentButtons: [
        { label: "엑셀 업로드", type: "table-header-default", size: "small" },
        { label: "엑셀 다운로드", type: "table-header-default", size: "small" },
        { label: "선택 삭제", type: "table-header-danger", size: "small" },
        { label: "등록", type: "table-header-primary", size: "small" },
      ],
      filters: [
        {
          model: "",
          type: "select",
          label: "사용 여부",
          options: [
            { value: "", label: "전체" },
            { value: "Y", label: "사용" },
            { value: "N", label: "미사용" },
          ],
        },
        {
          model: "",
          type: "text",
          label: "ID",
          placeholder: "사용자 혹은 등록자 ID 입력",
        },
        {
          model: "",
          type: "text",
          label: "이름",
          placeholder: "사용자 혹은 등록자 이름 입력",
        },
      ],
      rows: Array.from({ length: 28 }, (_, i) => ({
        id: i + 1,
        userId: "gildong_hong",
        userName: "홍길동",
        lastLogin: "2025.01.01 15:30",
        status: "사용",
        modifierId: "gildong_hong",
        modifierName: "홍길동",
        modifiedAt: "2025.01.01 15:30",
        note: "-",
      })),
      selectedRows: [],
      selectAll: false,
      currentPage: 1,
      rowsPerPage: 10,
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.rows.length / this.rowsPerPage);
    },
    paginatedRows() {
      const start = (this.currentPage - 1) * this.rowsPerPage;
      return this.rows.slice(start, start + this.rowsPerPage);
    },
  },
  methods: {
    toggleAllCheckboxes() {
      this.selectedRows = this.selectAll ? this.rows.map((row) => row.id) : [];
    },
    prevPage() {
      if (this.currentPage > 1) this.currentPage--;
    },
    nextPage() {
      if (this.currentPage < this.totalPages) this.currentPage++;
    },
    changePage(page) {
      this.currentPage = page;
    },
  },
};
</script>

<style scoped>
.content-area {
  padding: 0px 35px 35px 35px;
}

.content-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0px 20px 8.5px;
}
.content-header h5 {
  font-size: var(--font-size-base);
  color: var(--fontbk-color);
}

.content-buttons {
  display: flex;
  align-items: center;
  gap: 10px;
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
</style>
