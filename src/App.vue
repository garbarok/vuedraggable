<template>
  <div class="editor">
    <div class="canvas">
      <div v-for="area in areas" :key="area.name" :class="['area', `area-${area.name.toLowerCase()}`]">
        <div class="area-name">{{ area.name }}</div>
        <div class="area-content">
          <transition-group name="list-group" tag="div" class="list-group">
            <draggable :list="area.elements" :group="{ name: 'areas' }" ghost-class="ghost" :move="checkMove"
              :empty-insert-threshhold="500">
              <template #item="{ element, index }">
                <div class="element" :key="element.id">
                  <div class="icon">
                    <svg v-html="element.icon" class="icon-svg"></svg>
                  </div>
                  <div class="content">
                    {{ element.content }}
                  </div>
                </div>
              </template>
            </draggable>
          </transition-group>
        </div>
      </div>

    </div>
    <div class="sidebar">
      <div class="sidebar-title">Elements <a href="https://github.com/garbarok/vuedraggable" target="_blank"
          class="github-icon">
          <i class="fab fa-github"></i>
        </a></div>

      <SidebarElements :sidebarElements="sidebarElements" :move="checkMove" />
    </div>
  </div>
</template>




<script>
import draggable from "vuedraggable";
import sidebarElements from './data/sidebarElements.js';
import SidebarElements from './components/SidebarElements.vue';

export default {
  components: {
    draggable,
    SidebarElements
  },
  data() {
    return {
      sidebarElements: sidebarElements,
      areas: [
        { name: 'Header', elements: [] },
        { name: 'Body', elements: [] },
        { name: 'Footer', elements: [] }
      ]
    };
  },
  methods: {
    checkMove: function (evt) {
      const toAreaName = evt.to.closest('.area').classList[1].split('-')[1];
      const draggedElementType = evt.draggedContext.element.type;
      if (toAreaName === 'header' && draggedElementType !== 'image') {
        return false;
      }
      if (toAreaName === 'footer' && draggedElementType !== 'text') {
        return false;
      }
      return true;
    }
  }
};
</script>

<style scoped>
svg {
  width: 43px;
  height: 43px;
}

.editor {
  display: flex;
}

.sidebar {
  width: 450px;
  overflow-y: auto;
  display: flex;
  padding: 32px;
  flex-direction: column;
  align-items: flex-start;
  gap: 32px;
  border-left: 1px solid var(--neutral-neutral-200, #E9E9E9);
  background: var(--neutral-neutral-50, #FAFAFA);
  min-height: 100vh;
}

.sidebar-title {
  color: var(--secondary-secondary-400, #3a6b88);
  text-align: left;
  font: var(--heading-medium-semi-bold, 600 18px "Montserrat", sans-serif);
  position: relative;
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: center;
}

.github-icon {
  font-size: 24px;
  color: #333;
}

.github-icon:hover {
  color: #4078c0;
}

.list-group:empty,
.list-group>div:empty {
  min-height: 180px;
  text-align: center;
  color: #888;
  font-style: italic;
  display: flex;
  height: auto;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  align-self: stretch;
  border-radius: 2px;
  border: 1px dashed var(--secondary-secondary-400, #3A6B88);
  background: var(--neutral-neutral-00, #FFF);
  color: var(--neutral-neutral-900, #3F3F3F);
  font-family: Inter;
  font-size: 14px;
  font-style: normal;
  font-weight: 400;
}

.list-group:empty:before,
.list-group>div:empty:before {
  content: 'Drag and drop an element within this area.';
}

.ghost {
  opacity: 0.5;
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

.canvas {
  display: flex;
  flex-direction: column;
  width: calc(100% - 450px);
  overflow-y: auto;
  padding: 10px 6%;
}

.area-header,
.area-body,
.area-footer {
  min-height: 100px;
  border: 1px solid #ccc;
  margin-bottom: 10px;
  padding: 10px;
}

.element {
  margin: 5px;
  text-align: center;

}

.area-name {
  font-weight: bold;
  margin-bottom: 16px;
  color: var(--secondary-secondary-400, #3A6B88);
  font-family: Inter;
  font-size: 14px;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}

.area {
  padding: 16px 8px;
  border-radius: 4px;
  background: var(--neutral-neutral-100, #F6F6F6);
}

.area-content {
  line-height: normal;

}


.sortable-chosen {
  position: relative !important;
  /* Enables absolute positioning within parent container */
  left: 50%;
  /* Center horizontally */
  transform: translateX(-50%);
  /* Offset by half of the element's width */
  max-width: 100%;
  /* Allows the element to take up the full width of its container */
  min-width: 50px;
  /* Sets a minimum width */
  width: auto;
  /* Sizes the element according to its content */
}
</style>