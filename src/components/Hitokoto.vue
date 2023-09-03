<template>
  <div class="hitokoto cards" v-show="!store.musicOpenState" @mouseenter="openMusicShow = true"
    @mouseleave="openMusicShow = false" @click.stop>
    <!-- 打开音乐面板 -->
    <Transition name="el-fade-in-linear">
      <div class="open-music" v-show="openMusicShow && store.musicIsOk" @click="store.musicOpenState = true">
        <music-menu theme="filled" size="18" fill="#efefef" />
        <span>打开音乐播放器</span>
      </div>
    </Transition>
    <!-- 新内容：时间胶囊 -->
    <div class="time-capsule">
      <span class="text">今日已经度过了&nbsp;{{ timeData.day.elapsed }}&nbsp;小时</span>
      <el-progress :text-inside="true" :stroke-width="15" :percentage="timeData.day.pass" />
      <span class="text">本周已经度过了&nbsp;{{ timeData.week.elapsed }}&nbsp;天</span>
      <el-progress :text-inside="true" :stroke-width="15" :percentage="timeData.week.pass" />
      <span class="text">本月已经度过了&nbsp;{{ timeData.month.elapsed }}&nbsp;天</span>
      <el-progress :text-inside="true" :stroke-width="15" :percentage="timeData.month.pass" />
      <span class="text">今年已经度过了&nbsp;{{ timeData.year.elapsed }}&nbsp;个月</span>
      <el-progress :text-inside="true" :stroke-width="15" :percentage="timeData.year.pass" />

    </div>
  </div>
</template>

<script setup>
import { MusicMenu, HourglassFull } from "@icon-park/vue-next";
import { getTimeCapsule, siteDateStatistics } from "@/utils/getTime.js";
import { mainStore } from "@/store";
import debounce from "@/utils/debounce.js";
const store = mainStore();

// 进度条数据
const timeData = ref(getTimeCapsule());
const startDate = ref(import.meta.env.VITE_SITE_START);
const startDateText = ref(null);
const timeInterval = ref(null);

// 开启音乐面板按钮显隐
const openMusicShow = ref(true);

const getHitokotoData = () => {
  // ... (Your existing hitokoto fetching code)
};

const updateHitokoto = () => {
  // ... (Your existing hitokoto update code)
};

onMounted(() => {
  getHitokotoData();

  timeInterval.value = setInterval(() => {
    timeData.value = getTimeCapsule();
    if (startDate.value) startDateText.value = siteDateStatistics(new Date(startDate.value));
  }, 1000);
});

onBeforeUnmount(() => {
  clearInterval(timeInterval.value);
});
</script>

<style lang="scss" scoped>
.time-capsule {
  width: 100%;
  background: #00000050;
  padding: 4px 0;

  .title {
    display: flex;
    flex-direction: row;
    align-items: center;
    margin: 0.2rem 0 1.5rem;
    font-size: 1.1rem;

    .i-icon {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-right: 1px;
    }
  }

  .text {
    display: block;
    margin: 1rem 0rem 0.5rem 0rem;
    font-size: 0.6rem;
  }


  /* Reduce border width by 30% */
  .el-progress {
    border-width: 8px;
    /* You can adjust this value based on your design */
  }

  .el-progress-bar__innerText {
    font-size: 2px;
    /* 您可以根据需要调整字体大小 */
    color: white;
  }


}
</style>
