<template>
  <div class="container">
    <RouterView />
    <ThemeToggle />
  </div>
</template>

<script setup lang="ts">
import { useThemeStore } from '@/stores/theme';
import ThemeToggle from '@/components/ui/ThemeToggle.vue';

const themeStore = useThemeStore();
if (themeStore.isDark) {
  document.documentElement.setAttribute('data-theme', 'dark');
}
</script>
