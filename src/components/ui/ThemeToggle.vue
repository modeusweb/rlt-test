<template>
  <app-button @click="toggleTheme" styling="icon" class="theme-toggle">
    <span v-if="!isDark">🌙</span>
    <span v-else>☀️</span>
  </app-button>
</template>

<script setup lang="ts">
import { computed } from 'vue';
import { useThemeStore } from '@/stores/theme';
import AppButton from '@/components/ui/AppButton.vue';

const themeStore = useThemeStore();

const toggleTheme = () => {
  themeStore.toggleTheme();
};

const isDark = computed(() => themeStore.isDark);
</script>

<style scoped lang="scss">
.theme-toggle {
  position: fixed;
  right: 20px;
  top: 20px;
  font-size: 24px;
  z-index: 999;
}
</style>
