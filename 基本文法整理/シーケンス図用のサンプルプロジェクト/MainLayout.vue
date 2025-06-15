<script setup lang="ts">
import { defineEmits, defineProps } from 'vue';

const props = defineProps<{
  showHeader: boolean
  showSubArea: boolean
  showFooter: boolean
  headerHeight: string
  subAreaWidth: string
  footerHeight: string
}>()

const emit = defineEmits<{
  (e: 'update:showSubArea', value: boolean): void
}>()
</script>

<template>
  <v-container fluid>
    <!-- Header -->
    <v-row v-if="showHeader" :style="`height: ${headerHeight}`">
      <v-col>
        <slot name="header" />
      </v-col>
    </v-row>

    <!-- SubArea + Main -->
    <v-row no-gutters>
      <!-- ✅ SubArea -->
      <v-col
        v-if="showSubArea"
        :style="`width: ${subAreaWidth}; position: relative;`"
      >
        <!-- 🔘 閉ボタン -->
        <v-btn
          icon
          size="small"
          style="position: absolute; top: 4px; right: 4px; z-index: 1;"
          @click="emit('update:showSubArea', false)"
        >
          <v-icon>mdi-chevron-left</v-icon>
        </v-btn>

        <!-- Slot -->
        <slot name="subarea" />
      </v-col>

      <!-- ✅ subAreaが非表示なら「開」ボタンだけ表示 -->
      <v-col
        v-else
        class="d-flex align-start"
        :style="`width: 40px; position: relative;`"
      >
        <v-btn
          icon
          size="small"
          style="position: absolute; top: 4px; left: 4px; z-index: 1;"
          @click="emit('update:showSubArea', true)"
        >
          <v-icon>mdi-chevron-right</v-icon>
        </v-btn>
      </v-col>

      <!-- ✅ メインエリア -->
      <v-col>
        <slot name="main" />
      </v-col>
    </v-row>

    <!-- Footer -->
    <v-row v-if="showFooter" :style="`height: ${footerHeight}`">
      <v-col>
        <slot name="footer" />
      </v-col>
    </v-row>
  </v-container>
</template>
