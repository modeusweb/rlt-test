<template>
  <div class="inventory">
    <grid-layout
      class="inventory__cells"
      :layout="layout"
      :col-num="colsQty"
      :max-rows="rowsQty"
      :row-height="100"
      :margin="[0, 0]"
      :is-draggable="true"
      :is-resizable="false"
      :vertical-compact="false"
      :prevent-collision="true"
      :useStyleCursor="false"
      @layout-updated="updateLayout"
    >
      <grid-item
        v-for="item in items"
        class="inventory__cell"
        :key="item.id"
        :x="item.x"
        :y="item.y"
        :w="1"
        :h="1"
        :i="item.id"
      >
        <InventoryItem :item="item" />
      </grid-item>
    </grid-layout>
    <div class="inventory__grid">
      <div
        v-for="i in rowsQty * colsQty"
        :key="i"
        class="inventory__grid-cell"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { useInventoryStore } from '@/stores/inventory';
import { GridLayout, GridItem } from 'vue-grid-layout-v3';
import InventoryItem from '@/components/InventoryItem.vue';

const inventoryStore = useInventoryStore();
const items = inventoryStore.items;

const layout = ref(
  items.map((item) => ({
    x: item.x,
    y: item.y,
    w: 1,
    h: 1,
    i: item.id,
  })),
);

const colsQty = 5;
const rowsQty = 5;

const updateLayout = (newLayout: any) => {
  newLayout.forEach((layoutItem: any) => {
    const item = items.find((item) => item.id === layoutItem.i);
    if (item) {
      item.x = layoutItem.x;
      item.y = layoutItem.y;
    }
  });
  inventoryStore.saveToLocalStorage();
};

watch(
  () => inventoryStore.items,
  (newItems) => {
    layout.value = newItems.map((item) => ({
      x: item.x,
      y: item.y,
      w: 1,
      h: 1,
      i: item.id,
    }));
  },
);
</script>

<style scoped lang="scss">
.inventory {
  min-height: 500px;
  position: relative;

  &__cell {
    display: flex;
    justify-content: center;
    align-items: center;

    &.vue-draggable-dragging {
      border: 1px solid $primary-border-color;
      border-radius: $large-border-radius;
      overflow: hidden;
    }
  }

  &__grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    gap: 0;
    position: absolute;
    left: 0;
    right: -1px;
    top: 0;
    bottom: -2px;
    pointer-events: none;

    &-cell {
      border-right: 1px solid $primary-border-color;
      border-bottom: 1px solid $primary-border-color;
    }
  }
}
</style>
