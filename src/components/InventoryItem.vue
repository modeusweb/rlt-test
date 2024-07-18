<template>
  <div class="inventory-item">
    <div class="inventory-item__image">
      <img :src="item.image" :alt="item.name" />
    </div>
    <div class="inventory-item__quantity">{{ item.quantity }}</div>
  </div>
</template>

<script setup lang="ts">
import type { InventoryItem } from '@/interfaces/InventoryItem';

defineProps<{ item: InventoryItem }>();
</script>

<style scoped lang="scss">
.inventory-item {
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  width: 100%;
  height: 100%;
  padding: 5px;
  cursor: pointer;
  user-select: none;
  transition: background-color 0.3s;

  &:hover {
    background-color: $light-background-color;
  }

  &__image {
    display: flex;
    img {
      max-width: 100%;
      max-height: 100%;
      object-fit: contain;
    }
  }

  &__quantity {
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 16px;
    height: 18px;
    padding: 0 5px;
    border: 1px solid $primary-border-color;
    border-right-color: transparent;
    border-bottom-color: transparent;
    position: absolute;
    right: 0;
    bottom: 0;
    border-radius: 6px 0 0 0;
    background-color: $secondary-background-color;
    font-size: 10px;
    line-height: 1;
    text-align: center;
    white-space: nowrap;
    color: rgba(255, 255, 255, 0.4);
  }
}

.vue-draggable-dragging {
  .inventory-item {
    background: $secondary-background-color;

    &__quantity {
      opacity: 0;
    }
  }
}

// light theme styling
html[data-theme='light'] {
  .inventory-item {
    &:hover {
      background: #efefef;
    }
    &__quantity {
      background: #fff;
      color: $dark-text-color;
    }
  }
}
</style>
