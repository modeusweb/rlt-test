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
        <app-button @click="toggleDeleteConfirm">Удалить предмет</app-button>
      </div>
    </template>
    <app-button
      class="item-details__close"
      styling="icon"
      @click="closeDetails"
    >
      <app-icon name="close" />
    </app-button>

    <!-- Всплывающий блок подтверждения удаления -->
    <transition name="slide-up">
      <form
        v-if="showDeleteConfirm"
        class="delete-confirm"
        @submit.prevent="confirmDelete"
      >
        <app-input
          id="delete-quantity"
          type="number"
          v-model.number="deleteQuantity"
          min="1"
          :max="item?.quantity"
          name="delete-quantity"
          placeholder="Введите количество"
          :required="true"
        />
        <div class="delete-confirm__buttons">
          <app-button
            @click="toggleDeleteConfirm"
            styling="secondary"
            size="small"
            type="button"
          >
            Отмена
          </app-button>
          <app-button size="small" type="submit">Подтвердить</app-button>
        </div>
      </form>
    </transition>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { Skeletor } from 'vue-skeletor';
import { useInventoryStore } from '@/stores/inventory';
import type { InventoryItem } from '@/interfaces/InventoryItem';
import AppIcon from '@/components/ui/AppIcon.vue';
import AppButton from '@/components/ui/AppButton.vue';
import AppInput from '@/components/ui/AppInput.vue';

const props = defineProps<{ item: InventoryItem | null }>();
const emits = defineEmits(['close']);

const inventoryStore = useInventoryStore();

const deleteQuantity = ref<string>('');
const showDeleteConfirm = ref<boolean>(false);

const closeDetails = () => {
  emits('close');
};

const toggleDeleteConfirm = () => {
  showDeleteConfirm.value = !showDeleteConfirm.value;
};

const resetInput = () => {
  deleteQuantity.value = '';
};

const confirmDelete = () => {
  if (props.item) {
    inventoryStore.removeItem(props.item.id, +deleteQuantity.value);
    toggleDeleteConfirm();
    emits('close');
    resetInput();
  }
};
</script>

<style scoped lang="scss">
.item-details {
  display: flex;
  flex-direction: column;
  width: 250px;
  position: absolute;
  right: -100%;
  top: 0;
  bottom: 0;
  padding: 24px 14px 17px;
  border-left: 1px solid $primary-border-color;
  color: white;
  z-index: 99;
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  background: rgba(38, 38, 38, 0.5);
  text-align: center;
  transition: right 0.4s ease;

  // hack for safari
  @supports (font: -apple-system-body) and (-webkit-appearance: none) {
    background: $secondary-background-color;
  }

  &.opened {
    right: 0;
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
    max-height: 114px;
    overflow: auto;
  }

  &__param {
    flex: 0 0 10px;
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

.delete-confirm {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  padding: 20px 21px;
  border-top: 1px solid $primary-border-color;
  background: $secondary-background-color;
  color: white;
  z-index: 100;
  text-align: center;

  p {
    margin-bottom: 10px;
  }

  input {
    width: 100%;
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }

  &__buttons {
    display: flex;
    gap: 10px;
    justify-content: center;
    margin-top: 20px;

    .btn {
      flex: 1;
    }
  }
}

.slide-up-enter-active,
.slide-up-leave-active {
  transition: transform 0.3s ease;
}

.slide-up-enter-from,
.slide-up-leave-to {
  transform: translateY(100%);
}
</style>
