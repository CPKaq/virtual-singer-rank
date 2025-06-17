<script setup>
import { ref } from 'vue'
import { CdxButton, CdxTextInput, CdxProgressBar, CdxTable } from '@wikimedia/codex'

const inputValue = ref(0);     // 输入的期号
const isLoading = ref(false);  // 加载状态
const tableCaption = ref(' ');      // 表格标题

const columns = [
  { id: 'volume', label: '期号', textAlign: 'number'},
  { id: 'ranking', label: '排名', textAlign: 'number'},
  { id: 'id', label: 'av号', textAlign: 'number'},
  { id: 'point', label: '最终得点', textAlign: 'number'},
  { id: 'view', label: '播放', textAlign: 'number'},
  { id: 'reply', label: '评论', textAlign: 'number'},
  { id: 'danmaku', label: '弹幕', textAlign: 'number'},
  { id: 'favorite', label: '收藏', textAlign: 'number'},
  { id: 'corrA', label: '修正A', textAlign: 'number'},
  { id: 'corrB', label: '修正B', textAlign: 'number'}
];

const data = ref([]);
  
async function searchByVolume(volume) {
  data.value = [];
  isLoading.value = true;
  tableCaption.value = ' ';

  try {
    const response = await fetch(`https://vc-weekly.cpk.moe/api?volume=${volume}`);
    data.value = await response.json();
  } catch (error) {
    console.error('请求失败', error);
  } finally {
    isLoading.value = false;
    tableCaption.value = rankingTitle(volume);
  }
}

function rankingTitle(volume) {
  if (volume < 1) {
    return ' ';
  } else if (volume < 54) {
    return `洛天依新曲排行榜%${volume}`;
  } else if (volume < 118) {
    return `中文VOCALOID新曲排行榜%${volume}`;
  } else if (volume < 523) {
    return `周刊VOCALOID中文排行榜♪${volume}`;
  } else {
    return `周刊虚拟歌手中文曲排行榜♪${volume}`;
  }
}
</script>

<template>
  <div class="searchBar">
    <cdx-text-input v-model="inputValue" class="searchInput"></cdx-text-input>
    <cdx-button @click="searchByVolume(inputValue)">查询</cdx-button>
  </div>
  <div class="progressBar">
    <cdx-progress-bar v-if="isLoading" aria-label="ProgressBar" />
  </div>
  <div class="searchResult">
    <cdx-table
    caption="查询结果"
      :columns="columns"
		  :data="data"
      :header
    >
      <template #header>
        {{ tableCaption }}
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

.searchResult {
  white-space: nowrap;
}
</style>