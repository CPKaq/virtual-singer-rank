<script setup>
import { ref } from 'vue'
import { CdxButton, CdxTextInput, CdxTable } from '@wikimedia/codex'

const inputValue = ref(0)

const columns = [
  { id: 'volume', label: '期号', textAlign: 'number'},
  { id: 'ranking', label: '排名', textAlign: 'number'},
  { id: 'id', label: 'av号', textAlign: 'number'},
  { id: 'point', label: '最终得点', textAlign: 'number'},
  { id: 'view', label: '播放', textAlign: 'number'},
  { id: 'reply', label: '评论', textAlign: 'number'},
  { id: 'danmaku', label: '弹幕', textAlign: 'number'},
  { id: 'favorite', label: '收藏数', textAlign: 'number'},
  { id: 'corrA', label: '修正A', textAlign: 'number'},
  { id: 'corrB', label: '修正B', textAlign: 'number'}
];

const data = ref([]);
  
async function searchByVolume(volume) {
  try {
    const response = await fetch(`https://vc-weekly.cpk.moe/api?volume=${volume}`);
    this.data = await response.json();
  } catch (error) {
    console.error('请求失败', error);
  }
}
</script>

<template>
  <div class="searchBar">
    <cdx-text-input v-model="inputValue" class="searchInput"></cdx-text-input>
    <cdx-button @click="searchByVolume(inputValue)">查询</cdx-button>
  </div>
  <div class="searchResult">
    <cdx-table
    caption="查询结果"
      :columns="columns"
		  :data="data"
      :header
    >
      <template #header>
        周刊VOCALOID中文排行榜♪{{ inputValue }}
      </template>
    </cdx-table>
  </div>
</template> 

<style scoped>
.searchBar {
  display: flex;
  flex-direction: row;
  align-items: center;
  /* gap: 1em; */
  width: 100%;
  margin: 1em 0;
}

.searchInput {
  width: 100%;
}
</style>