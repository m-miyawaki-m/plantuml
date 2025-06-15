<script setup lang="ts">
import { ref } from 'vue'
import ModalComponent from './ModalComponent.vue'
import TabSwitchComponent from './TabSwitchComponent.vue'

const listItems = ref([
  { id: 1, name: '項目A' },
  { id: 2, name: '項目B' },
  { id: 3, name: '項目C' },
])

const menuIndex = ref<number | null>(null)
const menuActivatorRefs = ref<HTMLElement[]>([])
const showModal = ref(false)
const showTabSwitch = ref(false)

const openMenu = (index: number, event: MouseEvent) => {
  menuIndex.value = index
  // 各行ごとの ref を保持している前提
  menuActivatorRefs.value[index]?.click()
}
</script>

<template>
  <v-list>
    <v-list-item
      v-for="(item, index) in listItems"
      :key="item.id"
      @click="openMenu(index, $event)"
    >
      <v-list-item-title>{{ item.name }}</v-list-item-title>

      <!-- メニューのアクティベータを ref に記憶 -->
      <template #append>
        <v-menu
          :activator="menuActivatorRefs[index]"
          v-model="menuIndex === index"
          location="bottom"
        >
          <v-list>
            <v-list-item @click="showModal = true">
              <v-list-item-title>モーダル表示</v-list-item-title>
            </v-list-item>
            <v-list-item @click="showTabSwitch = true">
              <v-list-item-title>タブ切り替え表示</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
        <span
          ref="el => menuActivatorRefs[index] = el"
          style="display: none"
        />
      </template>
    </v-list-item>
  </v-list>

  <!-- モーダル表示 -->
  <ModalComponent v-model="showModal" />

  <!-- v-window切り替え -->
  <TabSwitchComponent v-if="showTabSwitch" />
</template>
