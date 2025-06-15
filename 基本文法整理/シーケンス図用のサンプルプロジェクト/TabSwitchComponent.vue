<script setup lang="ts">
import { ref } from 'vue';

const activeTab = ref(0)
</script>

<template>
  <v-card>
    <v-tabs v-model="activeTab">
      <v-tab value="0">タブA</v-tab>
      <v-tab value="1">タブB</v-tab>
    </v-tabs>

    <v-window v-model="activeTab">
      <v-window-item value="0">
        <v-card-text>タブAの内容</v-card-text>
      </v-window-item>
      <v-window-item value="1">
        <v-card-text>タブBの内容</v-card-text>
      </v-window-item>
    </v-window>
  </v-card>
</template>
