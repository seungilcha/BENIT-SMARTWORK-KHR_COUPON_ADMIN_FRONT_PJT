<template>
  <aside class="sidebar">
    <div class="menu-container">
      <div class="menu-title">Menu</div>
      <ul class="nav-items">
        <li v-for="(item, index) in navItems" :key="index" class="nav-item">
          <!-- 📌 1 Depth 메뉴 -->
          <div
            class="nav-link"
            :class="{ active: activeIndex === index }"
            @click="handleMenuClick(index, item)"
          >
            <span class="nav-text">{{ item.text }}</span>
            <svg
              class="arrow-icon"
              :class="{ rotated: activeIndex === index }"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 20 20"
              fill="none"
            >
              <path d="M10 14L14.3301 9H5.66987L10 14Z" fill="#1C1C1C" />
            </svg>
          </div>

          <!-- 📌 2 Depth 메뉴 -->
          <transition name="slide">
            <ul v-if="activeIndex === index" class="sub-nav-items">
              <li
                v-for="(subItem, subIndex) in item.subMenu"
                :key="subIndex"
                class="sub-nav-item"
              >
                <a
                  href="#"
                  class="sub-nav-link"
                  :class="{ 'sub-active': selectedSubMenu === subItem }"
                  @click.prevent="
                    handleSubMenuClick(
                      subItem,
                      item.subMenuFiles[subIndex],
                      item.fileName
                    )
                  "
                >
                  {{ subItem }}
                </a>
              </li>
            </ul>
          </transition>
        </li>
      </ul>
    </div>
    <div class="btn-cover">
      <BaseButton type="default" size="small">로그아웃</BaseButton>
    </div>
  </aside>
</template>

<script>
import BaseButton from "@/components/UI/BaseButton.vue"; // ✅ BaseButton 가져오기
export default {
  components: {
    BaseButton, // ✅ BaseButton 등록
  },
  emits: ["update-content"], // ✅ App.vue로 데이터 전달을 위한 이벤트 정의
  data() {
    return {
      activeIndex: null, // 🔹 현재 활성화된 1 Depth 메뉴 인덱스
      selectedSubMenu: null, // 🔹 현재 선택된 2 Depth 메뉴
      navItems: [
        {
          text: "시스템 관리",
          fileName: "SysMgmt",
          subMenu: [
            "관리자 관리",
            "관리자 메뉴 관리",
            "로그관리",
            "게시판 관리",
            "언어관리",
          ],
          subMenuFiles: [
            "page_Admin",
            "page_AdminMenu",
            "page_Logs",
            "page_Board",
            "page_Lang",
          ],
        },
        {
          text: "사이트 관리",
          fileName: "SiteMgmt",
          subMenu: [
            "코드 관리",
            "사용자 메뉴 관리",
            "메인 관리",
            "약관 관리",
            "그룹사 사이트 관리",
            "지도 관리",
          ],
          subMenuFiles: [
            "page_Code",
            "page_UserMenu",
            "page_Main",
            "page_Terms",
            "page_GroupSite",
            "page_Map",
          ],
        },
        {
          text: "게시판 관리",
          fileName: "BoardMgmt",
          subMenu: ["게시물관리", "미디어센터 관리"],
          subMenuFiles: ["page_Posts", "page_Media"],
        },
        {
          text: "배너 관리",
          fileName: "BannerMgmt",
          subMenu: ["배너 목록", "배너 등록/수정", "배너 삭제"],
          subMenuFiles: ["page_List", "page_Edit", "page_Delete"],
        },
        {
          text: "팝업 관리",
          fileName: "PopupMgmt",
          subMenu: ["팝업 목록", "팝업 등록/수정", "팝업 삭제"],
          subMenuFiles: ["page_List", "page_Edit", "page_Delete"],
        },
        {
          text: "배치/스케줄링 관리",
          fileName: "BatchMgmt",
          subMenu: ["배치/스케줄링 실행 내역 조회", "배치/스케줄링 수동 실행"],
          subMenuFiles: ["page_History", "page_Manual"],
        },
        {
          text: "알림 관리",
          fileName: "NotifyMgmt",
          subMenu: ["알림 내용 등록", "알림 전송 내역 조회"],
          subMenuFiles: ["page_Create", "page_History"],
        },
        {
          text: "메일컨텐츠 관리",
          fileName: "MailMgmt",
          subMenu: [
            "메일내용 목록",
            "메일내용 등록",
            "메일내용 삭제",
            "메일 발신관리",
          ],
          subMenuFiles: [
            "page_List",
            "page_Create",
            "page_Delete",
            "page_Send",
          ],
        },
        {
          text: "마케팅/홍보 관련",
          fileName: "Marketing",
          subMenu: ["SEO", "google analytics"],
          subMenuFiles: ["page_SEO", "page_GA"],
        },
        {
          text: "승인",
          fileName: "Approval",
          subMenu: null, // 🔹 2 Depth가 없으면 바로 페이지 로드
        },
        {
          text: "이미지 관리",
          fileName: "ImgMgmt",
          subMenu: null, // 🔹 2 Depth가 없으면 바로 페이지 로드
        },
      ],
    };
  },
  created() {
    // 📌 기본으로 `page_Admin`이 보이도록 설정
    const defaultFolder = "SysMgmt";
    const defaultComponent = "page_Admin";
    this.activeIndex = this.navItems.findIndex(
      (item) => item.fileName === defaultFolder
    );
    this.selectedSubMenu = "관리자 관리";

    this.$emit("update-content", {
      folderName: defaultFolder,
      componentName: defaultComponent,
    });
  },
  methods: {
    /**
     * 1 Depth 메뉴 클릭 시 동작
     * @param {Number} index - 클릭한 메뉴의 인덱스
     * @param {Object} item - 선택한 메뉴 객체
     */
    handleMenuClick(index, item) {
      if (item.subMenu) {
        this.activeIndex = this.activeIndex === index ? null : index; // 🔹 메뉴 열고 닫기
      } else {
        this.activeIndex = index;
        this.selectedSubMenu = null; // 🔹 2 Depth 선택 해제
        this.$emit("update-content", {
          folderName: item.fileName,
          componentName: item.fileName,
        });
      }
    },

    /**
     * 2 Depth 메뉴 클릭 시 동작
     * @param {String} subItem - 선택한 서브 메뉴명
     * @param {String} fileName - 해당하는 Vue 파일명
     * @param {String} parentFileName - 1 Depth의 fileName (폴더명)
     */
    handleSubMenuClick(subItem, fileName, parentFileName) {
      this.selectedSubMenu = subItem; // 🔹 현재 선택된 2 Depth 메뉴 저장
      this.$emit("update-content", {
        folderName: parentFileName,
        componentName: fileName,
      });
    },
  },
};
</script>

<style scoped>
/* 📌 네비게이션 바 */
.sidebar {
  width: 284px;
  background-color: var(--white-color);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-right: 1px solid var(--lineStroke-color);
  padding: 20px 0;
}
.menu-container {
  width: 284px;
}
/* 📌 메뉴 타이틀 */
.menu-title {
  font-size: var(--font-size-xl);
  font-weight: bold;
  margin: 9px 0 23px 27px;
}

/* 📌 1 Depth 메뉴 스타일 */
.nav-items {
  list-style-type: none;
  padding: 0;
  width: 100%;
}

.nav-item {
  width: 100%;
  text-align: center;
}

/* 📌 1 Depth 메뉴 링크 */
.nav-link {
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: #000;
  padding: 10px 28px;
  font-size: var(--font-size-base);
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

/* 🔹 선택된 1 Depth 메뉴 스타일 */
.nav-link.active {
  color: var(--fontbk-color);
}

/* 📌 2 Depth 메뉴 */
.sub-nav-items {
  list-style-type: none;
}

.sub-nav-item {
  text-align: left;
}

.sub-nav-link {
  color: var(--fontbk-color);
  font-size: var(--font-size-base);
  text-decoration: none;
  padding: 10px 28px;
  display: block;
  transition: background-color 0.2s;
}

/* 🔹 선택된 2 Depth 메뉴 스타일 */
.sub-nav-link.sub-active {
  background-color: var(--primary-color);
  color: var(--white-color);
}

/* 🔹 1 Depth 메뉴 화살표 회전 효과 */
.arrow-icon {
  transition: transform 0.3s ease-in-out;
}

/* 🔹 펼쳐진 1 Depth 메뉴 화살표 180도 회전 */
.arrow-icon.rotated {
  transform: rotate(180deg);
}

.btn-cover {
  margin: 21px 0px 0px 27px;
}
</style>
