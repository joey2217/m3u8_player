<template>
  <header class="p-2 h-12">
    <el-input
      v-model="value"
      placeholder="Please input m3u8Url"
      clearable
      @keydown.enter="onConfirm"
    >
      <template #append>
        <el-button @click="onConfirm"> ✔️ </el-button>
      </template>
    </el-input>
  </header>
  <main class="main">
    <Player :m3u8Url="m3u8Url" />
  </main>
</template>

<script lang="ts" setup>
import { ref } from 'vue'
import Player from './components/Player.vue'

const m3u8Url = ref('https://livefr.cgtn.com/1000f/prog_index.m3u8')
const value = ref('https://livefr.cgtn.com/1000f/prog_index.m3u8')
const onConfirm = () => {
  m3u8Url.value = value.value
}
</script>

<style scoped>
.main {
  height: calc(100vh - 48px);
}
</style>
