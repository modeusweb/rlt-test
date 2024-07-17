<template>
  <component
    :is="props.tag"
    :class="['btn', `btn--${styling}`, `btn--${size}`]"
    @click="$emit('click')"
  >
    <slot />
  </component>
</template>

<script setup lang="ts">
const props = defineProps({
  tag: {
    type: String,
    default: 'button',
  },
  styling: {
    type: String,
    default: 'primary',
  },
  size: {
    type: String,
    default: 'normal',
    validator(value) {
      return ['normal', 'small'].includes(value as string);
    },
  },
});

defineEmits(['click']);
</script>

<style scoped lang="scss">
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 39px;
  padding: 5px 15px 7px;
  font-family: $secondary-font;
  font-weight: normal;
  font-size: 14px;
  line-height: 1.2;
  text-align: center;
  color: #fff;
  border-radius: $secondary-border-radius;
  transition: 0.3s;

  &--primary {
    background-color: $secondary-color;

    &:hover {
      background-color: darken($secondary-color, 10%);
    }
  }

  &--small {
    min-height: 33px;
  }

  &--icon {
    min-height: 0;
    padding: 0;
    background-color: transparent;
    border-radius: 0;
  }
}
</style>
