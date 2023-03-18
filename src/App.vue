<script setup lang="ts">
// import { darkTheme } from 'naive-ui';
import { zhCN, dateZhCN } from 'naive-ui';

const locale = zhCN;
const dateLocale = dateZhCN;
</script>

<template>
  <n-config-provider wh-full :theme="null" :locale="locale" :date-locale="dateLocale">
    <n-message-provider>
      <router-view />
    </n-message-provider>
  </n-config-provider>
</template>

<style scoped></style>
