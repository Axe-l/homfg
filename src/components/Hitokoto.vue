<template>
  <div
    class="hitokoto cards"
    v-show="!store.musicOpenState"
    @mouseenter="openMusicShow = false"
    @mouseleave="openMusicShow = false"
    @click.stop
  >
    <!-- 打开音乐面板 -->
    <Transition name="el-fade-in-linear">
      <div
        class="open-music"
        v-show="openMusicShow && store.musicIsOk"
        @click="closeMusicPanel"
      >
        <music-menu theme="filled" size="18" fill="#efefef" />
        <span>打开音乐播放器</span>
      </div>
    </Transition>
    <!-- 一言内容 -->
    <div class="content" @click="updateHitokoto">
      <span class="text">{{ hitokotoData.text }}</span>
      <span class="from">-「&nbsp;{{ hitokotoData.from }}&nbsp;」</span>
    </div>
  </div>
</template>

<script setup>
import { MusicMenu, Error } from "@icon-park/vue-next";
import { getHitokoto } from "@/api";
import { mainStore } from "@/store";
import debounce from "@/utils/debounce.js";

const store = mainStore();

// 开启音乐面板按钮显隐
const openMusicShow = ref(false);

// 一言数据
const hitokotoData = reactive({
  text: "这里应该显示一句话",
  from: "無名",
});

// 获取一言数据
const getHitokotoData = () => {
  getHitokoto()
    .then((res) => {
      hitokotoData.text = res.hitokoto;
      hitokotoData.from = res.from;
    })
    .catch(() => {
      ElMessage({
        message: "一言获取失败",
        icon: h(Error, {
          theme: "filled",
          fill: "#efefef",
        }),
      });
    });
};

// 更新一言数据
const updateHitokoto = () => {
  hitokotoData.text = "新的一言正在赶来的路上";
  hitokotoData.from = "来源加载中";
  // 防抖
  debounce(() => {
    getHitokotoData();
  }, 500);
};

// 关闭音乐面板
const closeMusicPanel = () => {
  store.musicOpenState = false;
  resetHitokotoData();
};

// 重置一言数据
const resetHitokotoData = () => {
  hitokotoData.text = "这里应该显示一句话";
  hitokotoData.from = "無名";
};

onMounted(() => {
  getHitokotoData();
});
</script>

<style lang="scss" scoped>
/* 样式略 */
</style>
