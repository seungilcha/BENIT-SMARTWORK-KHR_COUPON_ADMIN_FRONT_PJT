<template>
  <div class="tree-checkbox">
    <!-- ✅ 1뎁스 (최상위) -->
    <div class="tree-header">
      <BaseToggle
        v-model="treeData.checked"
        @change="toggleParent(treeData)"
        variant="square"
      />
      {{ treeData.label }}
    </div>

    <!-- ✅ 2뎁스 리스트 -->
    <ul class="tree-list">
      <li
        v-for="(node, index) in treeData.children"
        :key="index"
        class="tree-node"
      >
        <!-- 2뎁스 체크박스 + 토글 -->
        <div class="node-header">
          <BaseToggle
            v-model="node.checked"
            @change="toggleParent(node)"
            variant="square"
          />
          {{ node.label }}
          <span @click="toggleNode(node)" class="toggle-icon">
            <svg
              v-if="node.expanded"
              width="20"
              height="20"
              viewBox="0 0 20 20"
            >
              <path
                d="M10 8.3335L14.3301 13.3335H5.66987L10 8.3335Z"
                fill="#1C1C1C"
              />
            </svg>
            <svg v-else width="20" height="20" viewBox="0 0 20 20">
              <path d="M10 14L14.3301 9H5.66987L10 14Z" fill="#1C1C1C" />
            </svg>
          </span>
        </div>

        <!-- ✅ 3/4뎁스 리스트 -->
        <ul v-if="node.expanded" class="sub-list sub-list-3d">
          <li v-for="(child, cIndex) in node.children" :key="cIndex">
            <div>
              <BaseToggle
                v-model="child.checked"
                @change="toggleParent(child)"
              />
              {{ child.label }}
            </div>

            <!-- ✅ 4뎁스 리스트 -->
            <ul v-if="child.children" class="sub-list sub-list-4d">
              <li v-for="(subChild, sIndex) in child.children" :key="sIndex">
                <BaseToggle
                  v-model="subChild.checked"
                  @change="toggleParent(subChild)"
                />
                {{ subChild.label }}
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";
import BaseToggle from "./BaseToggle.vue"; // ✅ 체크박스 컴포넌트 활용

// ✅ 트리 데이터 (4뎁스까지 지원)
const treeData = ref({
  label: "전체선택",
  checked: false,
  expanded: true,
  children: [
    {
      label: "1뎁스 A",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 A-1",
          checked: false,
          children: [
            { label: "3뎁스 A-1-1", checked: false },
            { label: "4뎁스 A-1-2", checked: false },
          ],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
    {
      label: "1뎁스 B",
      checked: false,
      expanded: true,
      children: [
        {
          label: "2뎁스 B-1",
          checked: false,
          children: [{ label: "3뎁스 B-1-1", checked: false }],
        },
      ],
    },
  ],
});

// ✅ 부모가 체크되면 모든 하위 자동 체크
const toggleParent = (node) => {
  if (node.children) {
    node.children.forEach((child) => {
      child.checked = node.checked;
      toggleParent(child); // 하위 노드도 체크
    });
  }
  updateParentState(node); // 부모 상태 업데이트 (역방향)
};

// ✅ 자식이 모두 체크되면 부모도 자동 체크 (역방향)
const updateParentState = (node) => {
  if (!node.parent) return; // 1뎁스이면 종료
  node.parent.checked = node.parent.children.every((child) => child.checked);
  updateParentState(node.parent); // 상위 부모까지 체크 상태 반영
};

// ✅ 2뎁스 클릭 시 3/4뎁스 열고 닫기
const toggleNode = (node) => {
  node.expanded = !node.expanded;
};
</script>

<style scoped>
.tree-header {
  align-items: center; /* 세로 중앙 정렬 */
  gap: 1px;
  font-size: var(--font-size-base);
  color: var(--primary-color);
  background-color: var(--LightGray-color);
  border-bottom: 1px solid var(--lineStroke-color);
  padding: 12px 30px 12px 30px;
}

.tree-list {
  list-style: none;
  padding: 0;
  color: var(--fontbk-color);
  padding: 11px 30px;
}
.tree-list {
  max-height: 775px;
  overflow-y: auto;
}
.node-header {
  display: flex;
  align-items: center; /* 체크박스와 텍스트를 한 줄로 정렬 */
  gap: 5px; /* 체크박스와 텍스트 사이 간격 */
  cursor: pointer;
  font-weight: bold;
  background-color: var(--secondary-color);
  padding: 10px;
  margin-bottom: 3px;
}
.sub-list-3d {
  padding: 10px 0px;
}
.sub-list > li {
  padding: 0px 0px 0px 45px;
  position: relative;
}
.sub-list-3d::after {
  content: "";
  position: absolute;
  width: 1px;
  height: calc(100% - 20px);
  left: 18px;
  top: -11px;
  border-left: 1px dashed #000000;
}
.sub-list > li > .sub-list > li {
  padding: 10px 0px 10px 20px;
  position: relative;
}
.base-toggle,
.toggle-icon span {
  transform: scale(0.8); /* 체크박스 크기 80% 축소 */
  transform-origin: left center; /* 왼쪽 정렬 유지 */
  float: left;
}
.toggle-icon {
  display: flex;
  align-items: center; /* 세로 중앙 정렬 */
  justify-content: center; /* 가로 중앙 정렬 */
  width: 20px; /* 아이콘 크기와 동일한 컨테이너 크기 설정 */
  height: 20px;
  margin-left: auto;
  cursor: pointer;
}
.sub-list-3d {
  position: relative;
}
.sub-list-3d::before {
  content: "";
  position: absolute;
  width: 24px;
  height: 1px;
  left: 21px;
  top: 20px;
  border-top: 1px dashed #000000;
}
.sub-list-4d > li {
  position: relative;
}
.sub-list-4d > li::before {
  content: "";
  position: absolute;
  width: 44px;
  height: 1px;
  left: -26px;
  top: 20px;
  border-top: 1px dashed #000000;
}
</style>
