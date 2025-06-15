<script setup lang="ts">
import FooterComponent from '@/components/common/FooterComponent.vue'
import HeaderComponent from '@/components/common/HeaderComponent.vue'
import MainLayout from '@/components/layouts/MainLayout.vue'
import MainContentComponent from '@/components/page/○○/MainContentComponent.vue'
import SubPanelComponent from '@/components/page/○○/SubPanelComponent.vue'
import { ref } from 'vue'

// v-model 用の表示制御
const showHeader = ref(true)
const showSubArea = ref(true)
const showFooter = ref(true)

const headerHeight = '64px'
const subAreaWidth = '300px'
const footerHeight = '48px'
</script>

<template>
  <MainLayout
    v-model:showHeader="showHeader"
    v-model:showSubArea="showSubArea"
    v-model:showFooter="showFooter"
    :headerHeight="headerHeight"
    :subAreaWidth="subAreaWidth"
    :footerHeight="footerHeight"
  >
    <template #header>
      <HeaderComponent />
    </template>

    <template #subarea>
      <SubPanelComponent />
    </template>

    <template #main>
      <MainContentComponent />
    </template>

    <template #footer>
      <FooterComponent />
    </template>
  </MainLayout>
</template>
