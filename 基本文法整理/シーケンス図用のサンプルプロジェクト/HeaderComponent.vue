<script setup lang="ts">
import { defineComponent } from 'vue';

defineComponent({
  name: 'HeaderComponent',
})
</script>

<template>
  <v-app-bar flat color="primary" dark>
    <v-app-bar-title>
      システムタイトル
    </v-app-bar-title>

    <v-spacer />

    <!-- 任意の操作ボタン -->
    <v-btn icon>
      <v-icon>mdi-bell</v-icon>
    </v-btn>

    <v-btn icon>
      <v-icon>mdi-account-circle</v-icon>
    </v-btn>
  </v-app-bar>
</template>
