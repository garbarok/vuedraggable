<template>
  <div class="sidebar-list">
    <draggable class="sidebar-list" :list="sidebarElements" :group="{ name: 'areas', pull: 'clone', put: false }"
      :move="move">
      <template #item="{ element, index }">
        <div class="element sidebar-element" :key="element.id">
          <div class="icon">
            <svg v-html="element.icon" class="icon-svg"></svg>
          </div>
          <div class="content">
            {{ element.content }}
          </div>
        </div>
      </template>
    </draggable>
  </div>
</template>

<script>
import draggable from "vuedraggable";

export default {
  components: {
    draggable
  },
  props: {
    sidebarElements: {
      type: Array,
      required: true
    },
    move: {
      type: Function,
      required: true
    }
  }
}
</script>

<style scoped>
svg {
  width: 43px;
  height: 43px;
}

.sidebar-list {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;

}

.element {
  background-color: #eee;
  margin-bottom: 10px;
  cursor: move;
  display: flex;
  width: auto;
  height: 110px;
  padding: 24px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 8px;
  flex-shrink: 0;
  border-radius: 4px;
  border: 1px solid var(--neutral-neutral-200, #E9E9E9);
  background: var(--neutral-neutral-00, #FFF);
  box-shadow: 0px 2px 6px 0px rgba(0, 0, 0, 0.13);
  color: var(--secondary-secondary-400, #3A6B88);
  font-family: Inter;
  font-size: 14px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}

.sidebar-element {
  width: 110px;
  padding: 0px;
}


.element {
  margin: 5px;
  text-align: center;

}
</style>