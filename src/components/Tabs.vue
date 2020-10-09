<template>
   <div>
    <div id="tabs__header">
      <div
        v-for="(tab, index) in tabs"
        :key="tab.id"
        @click="selectTab(tab.id)"
        :class="[{'tab__selected': (tab.id === selectedIndex)}, 'tab']"
      >
        <div class="tab-name">
          <span v-show="tab.isUnsaved">*</span>
          <span v-if="tab.name">{{ tab.name }}</span>
          <span v-else class="tab-untitled">{{ tab.tempName }}</span>
        </div>
        <div>
          <svg
            class="close-icon"
            @click.stop="closeTab(index)"
            width="10"
            height="10"
            viewBox="0 0 14 14"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M14 1.41L12.59 0L7 5.59L1.41 0L0 1.41L5.59 7L0 12.59L1.41 14L7 8.41L12.59 14L14 12.59L8.41 7L14 1.41Z"
              fill="#A2B1C6"/>
          </svg>
        </div>
      </div>
    </div>
    <tab-content
      v-for="(tab, index) in tabs"
      :key="tab.id"
      :id="tab.id"
      :init-name="tab.name"
      :tab-index="index"
    />
  </div>
</template>

<script>
import TabContent from '@/components/TabContent'

export default {
  components: {
    TabContent
  },
  data () {
    return {
    }
  },
  computed: {
    tabs () {
      return this.$store.state.tabs
    },
    selectedIndex () {
      return this.$store.state.currentTabId
    }
  },
  methods: {
    selectTab (id) {
      this.$store.commit('setCurrentTabId', id)
    },
    closeTab (index) {
      this.$store.commit('deleteTab', index)
    }
  }
}
</script>

<style>
#tabs__header {
  display: flex;
  margin: 0;
  max-width: 100%;
  overflow: hidden;
}
#tabs__header .tab {
  height: 36px;
  background-color: var(--color-bg-light);
  border-right: 1px solid var(--color-border-light);
  border-bottom: 1px solid var(--color-border-light);
  line-height: 36px;
  font-size: 14px;
  color: var(--color-text-base);
  padding: 0 12px;
  box-sizing: border-box;
  position: relative;
  max-width: 200px;
  display: flex;
  flex-shrink: 1;
  min-width: 0;
}
#tabs__header .tab-name {
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  flex-shrink: 1;
}

#tabs__header div:hover {
  cursor: pointer;
}

#tabs__header .tab__selected {
  color: var(--color-text-active);
  font-weight: 600;
  border-bottom: none;
}
#tabs__header .tab__selected:hover {
    cursor: default;
}
#tabs__header .tab__selected:before {
  content: '';
  position: absolute;
  width: 100%;
  height: 5px;
  background-color: var(--color-accent);
  top: 0;
  left: 0;
}

.close-icon {
  margin-left: 5px;
}

.close-icon:hover path {
  fill: var(--color-text-base);
  cursor: pointer;
}
</style>