<template>
  <div class="page-container">
    <!-- 상단 헤더 컴포넌트 -->
    <AdminHeader
      title="관리자 등록/관리"
      :tableHeaderButtons="tableHeaderButtons"
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

    <!-- TableMain.vue` 컴포넌트 -->
    <TableMain
      :title="'시스템관리자'"
      :tableHeaderButtons="tableHeaderButtons"
      :tableButtons="tableButtons"
      :unit="'명'"
      :tables="tableData"
      :currentPage="currentPage"
      :totalPages="totalPages"
      :rowsPerPage="rowsPerPage"
      @updateRowsPerPage="updateRowsPerPage"
    />
    <!-- 삭제 레이어 팝업 추가 -->
    <LayerPopup
      v-if="isPopupVisible"
      :isVisible="isPopupVisible"
      title="관리자 삭제"
      message="선택하신 관리자를 삭제하시겠습니까?"
      @close="isPopupVisible = false"
    />
    <!-- 엑셀 다운로드 레이어 팝업 추가 -->
    <ExcelPopup
      ref="excelPopupRef"
      :title="'엑셀 다운로드 사유'"
      @confirm="handleExcelDownload"
    />
    <!-- 엑셀 업로드 레이어 팝업 추가 -->
    <ExcelUploadPopup
      ref="excelUploadPopupRef"
      title="엑셀 업로드"
      @upload="handleExcelUpload"
    />
    <!-- ContentArea 내부에서 동적으로 page_Admin.vue를 로드 -->
    <ContentArea
      :folderName="'components/pages/SysMgmt'"
      :componentName="'page_Admin'"
    />
  </div>
</template>

<script setup>
import { ref, h, defineComponent, computed, watch, onMounted } from "vue";
import ContentArea from "../../Layout/ContentArea.vue";
import AdminHeader from "../../UI/AdminHeader.vue";
import FilterBox from "../../UI/FilterBox.vue";
import TableMain from "../../UI/Table/TableMain.vue";
import ExcelUploadPopup from "../../UI/popup/ExcelUploadPopup.vue";
import LayerPopup from "../../UI/popup/LayerPopup.vue";
import ExcelPopup from "../../UI/popup/ExcelDownPopup.vue";

const excelPopupRef = ref(null);
const isPopupVisible = ref(false);

// ✅ 엑셀 다운로드 버튼 클릭 핸들러 추가
const openExcelPopup = () => {
  if (excelPopupRef.value) {
    excelPopupRef.value.openPopup();
  }
};

// ✅ ExcelPopup에서 "다운로드" 버튼 클릭 시 처리할 로직 추가
const handleExcelDownload = (textareaContent) => {
  console.log("다운로드 요청 내용:", textareaContent);
  // 여기에서 textareaContent 활용하여 엑셀 다운로드 로직 처리
};

const excelUploadPopupRef = ref(null);

const openExcelUploadPopup = () => {
  excelUploadPopupRef.value.openPopup();
};

const handleExcelUpload = (file) => {
  console.log("업로드한 파일:", file);
  // 파일 업로드 API 호출 등 처리
};

const openDeletePopup = () => {
  isPopupVisible.value = true; // ✅ 팝업 열기
};
const currentPage = ref(1);
const totalPages = ref(1);
const rowsPerPage = ref(10);
const tableData = ref([]); // ✅ 테이블 데이터
// ✅ 테이블 데이터 (초기 값 + 동적 데이터)
const fetchData = () => {
  const totalItems = 50;
  totalPages.value = Math.ceil(totalItems / rowsPerPage.value);

  tableData.value = [
    {
      headers: [
        "",
        "사용자ID",
        "사용자 이름",
        "최종접속일",
        "사용 여부",
        "최종 수정자ID",
        "최종 수정자 이름",
        "최종 수정 일시",
        "비고",
        "",
      ],
      rows: Array.from({ length: rowsPerPage.value }, (_, i) => ({
        checked: false,
        data: [
          { type: "text", value: `user_${i + 1}` },
          { type: "text", value: "홍길동" },
          { type: "text", value: "2025.01.01 15:30" },
          { type: "text", value: i % 2 === 0 ? "사용" : "미사용" },
          { type: "text", value: `modifier_${i + 1}` },
          { type: "text", value: "관리자" },
          { type: "text", value: "2025.01.01 15:30" },
          { type: "text", value: "-" },
          [
            {
              type: "button",
              label: "삭제",
              action: openDeletePopup,
            },
            {
              type: "button",
              label: "등록",
            },
          ],
        ],
      })),
    },
  ];
};
watch(rowsPerPage, () => {
  fetchData();
});
// ✅ 초기 데이터 로드
fetchData();

// ✅ 데이터 변경 감지
watch(rowsPerPage, fetchData);
watch(currentPage, fetchData);

// ✅ 행 개수 변경 시 업데이트
const updateRowsPerPage = (value) => {
  rowsPerPage.value = value;
  currentPage.value = 1;
};

// ✅ 전체 선택 체크박스 로직
const allChecked = computed({
  get: () => tableData.value[0]?.rows.every((row) => row.checked) ?? false,
  set: (value) => {
    if (typeof value === "boolean") {
      tableData.value[0]?.rows.forEach((row) => (row.checked = value));
    }
  },
});

// ✅ 엑셀 업로드/다운로드 아이콘 (기존 선언된 아이콘이 있으면 아래부분은 제외)
const UploadIcons = defineComponent({
  render() {
    return h(
      "svg",
      { width: "14", height: "14", viewBox: "0 0 14 14", fill: "none" },
      [
        h("path", {
          d: "M1 13H13",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
        }),
        h("path", {
          d: "M7 1L7 10",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
        }),
        h("path", {
          d: "M2.5 4.6L7 1L11.5 4.6",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
          "stroke-linejoin": "round",
        }),
      ]
    );
  },
});

const DwloadIcon = defineComponent({
  render() {
    return h(
      "svg",
      { width: "14", height: "14", viewBox: "0 0 14 14", fill: "none" },
      [
        h("path", {
          d: "M1 1H13",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
        }),
        h("path", {
          d: "M7 13L7 4",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
        }),
        h("path", {
          d: "M2.5 9.4L7 13L11.5 9.4",
          stroke: "black",
          "stroke-width": "1.5",
          "stroke-linecap": "round",
          "stroke-linejoin": "round",
        }),
      ]
    );
  },
});
const isDeleteButtonActive = computed(() =>
  tableData.value[0]?.rows.some((row) => row.checked)
);
// ✅ 테이블 헤더 버튼
const tableHeaderButtons = computed(() => [
  {
    label: "엑셀 업로드",
    type: "table-header-default",
    size: "small",
    showBar: false,
    action: openExcelUploadPopup,
    icon: UploadIcons,
  },
  {
    label: "엑셀 다운로드",
    type: "table-header-default",
    size: "small",
    showBar: true,
    action: openExcelPopup,
    icon: DwloadIcon,
  },
  {
    label: "선택 삭제",
    type: isDeleteButtonActive.value
      ? "table-header-default"
      : "table-header-danger",
    size: "small",
    showBar: false,
    action: isDeleteButtonActive.value ? openDeletePopup : null, // ✅ "선택 삭제" 클릭 시 팝업 열기
  },
  {
    label: "등록",
    type: "table-header-primary",
    size: "small",
    showBar: false,
  },
]);

// ✅ 필터 설정
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

onMounted(() => {
  document.querySelectorAll("tbody tr").forEach((row) => {
    if (row.innerHTML.includes("미사용")) {
      row.classList.add("inactive-row"); // ✅ 클래스 추가
    }
  });
});
</script>

<style scoped>
.page-container {
  display: block;
}
</style>
