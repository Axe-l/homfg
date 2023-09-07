<template>
  <div class="weather" v-if="weatherData.adCode.city">
    <span>{{ weatherData.adCode.city }}</span>
    <span>No Weather Data</span>
  </div>
  <div class="weather" v-else>
    <span></span>
  </div>
</template>

<script setup>
import { getAdcode } from "@/api";

// 高德开发者 Key
const mainKey = import.meta.env.VITE_WEATHER_KEY;

// 天气数据
const weatherData = reactive({
  adCode: {
    city: null, // 城市
    adcode: null, // 城市编码
  }
});

// 获取城市信息
const getCityData = () => {
  if (mainKey) {
    getAdcode(mainKey)
      .then((res) => {
        weatherData.adCode = {
          city: res.city,
          adcode: res.adcode,
        };
      })
      .catch((err) => {
        console.error("地理位置获取失败:" + err);
      });
  }
};

onMounted(() => {
  // 调用获取城市信息
  getCityData();
});
</script>
