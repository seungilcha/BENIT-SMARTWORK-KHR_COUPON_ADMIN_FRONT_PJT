<template>
  <header class="header">
    <div class="left-section">
      <a href="#" class="link-area">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="16"
          height="15"
          viewBox="0 0 16 15"
          fill="none"
        >
          <rect x="0.8" y="0.8" width="14.4" height="1.44" fill="white" />
          <rect x="0.8" y="6.8" width="14.4" height="1.44" fill="white" />
          <rect x="0.8" y="12.8" width="14.4" height="1.44" fill="white" />
        </svg>
      </a>

      <h1 class="logo">
        <a href="/" @click.prevent="goToHome">Logo 영역</a>
      </h1>
    </div>

    <div class="search-box">
      <input
        type="text"
        v-model="searchQuery"
        placeholder="검색어를 입력하세요."
        class="search-input"
      />
      <button @click="search" class="search-btn">
        <svg
          xmlns="http://www.w3.org/2000/svg"
          width="24"
          height="24"
          viewBox="0 0 24 24"
          fill="none"
        >
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M15.1694 13.832H14.4383L14.1791 13.5821C15.1175 12.4937 15.6332 11.1041 15.6322 9.667C15.6322 8.47713 15.2794 7.31397 14.6183 6.32463C13.9573 5.33528 13.0177 4.56418 11.9184 4.10883C10.8191 3.65349 9.60943 3.53435 8.44241 3.76648C7.2754 3.99861 6.20343 4.57159 5.36206 5.41296C4.52069 6.25433 3.94771 7.3263 3.71558 8.49332C3.48344 9.66033 3.60258 10.87 4.05793 11.9693C4.51327 13.0686 5.28438 14.0082 6.27372 14.6692C7.26307 15.3303 8.42622 15.6831 9.6161 15.6831C11.1062 15.6831 12.4761 15.137 13.5312 14.23L13.7811 14.4892V15.2203L18.4089 19.8389L19.788 18.4598L15.1694 13.832ZM9.6161 13.832C7.31146 13.832 5.45109 11.9716 5.45109 9.667C5.45109 7.36237 7.31146 5.502 9.6161 5.502C11.9207 5.502 13.7811 7.36237 13.7811 9.667C13.7811 11.9716 11.9207 13.832 9.6161 13.832Z"
            fill="white"
          />
        </svg>
      </button>
    </div>

    <!-- 오른쪽 접속자 정보 -->
    <div class="user-info"><span class="user-name">홍길동</span> 님</div>
  </header>
</template>

<script setup>
import { ref } from "vue";

const searchQuery = ref("");

const goToHome = () => {
  window.location.href = "/";
};

const search = () => {
  console.log("검색 실행:", searchQuery.value);
};
</script>

<style scoped>
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #1d231f;
  width: 100%;
  min-width: 770px;
  padding: 11px 45px 11px 27px;
}

/* 왼쪽 링크 + 로고 영역 */
.left-section {
  display: flex;
  align-items: center;
  gap: 6px; /* 아이콘과 로고 사이 여백 */
}

/* 햄버거 아이콘 (링크 영역) */
.link-area {
  display: flex;
  align-items: center;
  justify-content: center;
  width: var(--spacing-lg);
  height: var(--spacing-lg);
}

.link-area svg {
  width: 14.4px;
  height: 13.44px;
}

/* 로고 */
.logo {
  font-size: var(--font-size-base);
  font-weight: bold;
}

.logo a {
  color: var(--white-color);
  text-decoration: none;
}

/* 중앙 검색 영역 */
.search-box {
  position: relative;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #303933;
  border-radius: var(--btn-radius);
  width: 500px;
  height: 40px;
  padding: 0 20px 0 25px;
}

.search-input {
  flex-grow: 1;
  height: 100%;
  background: transparent;
  border: none;
  color: var(--white-color);
  font-size: var(--font-size-base);
  outline: none;
}

.search-button {
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  padding: 0;
}

.search-button svg {
  width: var(--spacing-lg);
  height: var(--spacing-lg);
}

/* 오른쪽 사용자 정보 */
.user-info {
  font-size: 14px;
  color: var(--white-color);
}

.user-name {
  font-weight: bold;
}
</style>
