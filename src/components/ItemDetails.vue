<template>
  <div class="item-details">
    <template v-if="item">
      <div class="item-details__image">
        <img :src="item.image" :alt="item.name" />
      </div>
      <div class="item-details__desc">
        <Skeletor class="item-details__name" height="30" />
        <div class="item-details__params">
          <Skeletor class="item-details__param" width="100%" height="10" />
          <Skeletor class="item-details__param" width="100%" height="10" />
          <Skeletor class="item-details__param" width="100%" height="10" />
          <Skeletor class="item-details__param" width="85%" height="10" />
          <Skeletor class="item-details__param" width="38%" height="10" />
        </div>
      </div>
      <div class="item-details__btn">
        <app-button @click="removeItem">Удалить предмет</app-button>
      </div>
    </template>
    <app-button
      class="item-details__close"
      styling="icon"
      @click="closeDetails"
    >
      <app-icon name="close" />
    </app-button>
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import { Skeletor } from 'vue-skeletor';
import { useInventoryStore } from '@/stores/inventory';
import type { InventoryItem } from '@/interfaces/InventoryItem';
import AppIcon from '@/components/ui/AppIcon.vue';
import AppButton from '@/components/ui/AppButton.vue';

const props = defineProps<{ item: InventoryItem | null }>();
const emits = defineEmits(['close']);

const inventoryStore = useInventoryStore();

const removeItem = () => {
  inventoryStore.removeItem(props.item.id, props.item.quantity);
  emits('close');
};

const closeDetails = () => {
  emits('close');
};
</script>

<style scoped lang="scss">
.item-details {
  display: flex;
  flex-direction: column;
  width: 250px;
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  padding: 24px 14px 17px;
  border-left: 1px solid $primary-border-color;
  overflow: auto;
  color: white;
  z-index: 99;
  backdrop-filter: blur(16px);
  background: rgba(38, 38, 38, 0.5);
  text-align: center;
  transition: 0.4s ease;
  transform: translateX(300%);

  &.opened {
    transform: translateX(0);
  }

  &__image {
    display: flex;
    align-items: center;
    justify-content: center;
    height: 190px;
    padding: 30px 0;
    border-bottom: 1px solid $primary-border-color;

    > img {
      width: 100%;
      height: 100%;
      max-width: 130px;
      max-height: 130px;
    }
  }

  &__desc {
    display: flex;
    flex-direction: column;
    gap: 24px;
    padding: 16px 4px 24px;
    border-bottom: 1px solid $primary-border-color;
  }

  &__name {
    border-radius: $secondary-border-radius;
  }

  &__params {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 16px;
  }

  &__param {
    border-radius: $small-border-radius;
  }

  &__btn {
    margin-top: auto;
    padding-top: 18px;

    .btn {
      width: 100%;
    }
  }

  &__close {
    position: absolute;
    top: 13px;
    right: 13px;

    &:before {
      content: '';
      position: absolute;
      left: -10px;
      right: -10px;
      top: -10px;
      bottom: -10px;
    }

    svg {
      width: 12px;
      height: 12px;
    }
  }
}
</style>
