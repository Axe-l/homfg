
<template>
  <!-- 功能区域 -->
  <div :class="store.mobileFuncState ? 'function mobile' : 'function'">
    <el-row :gutter="20">
      <el-col :span="12">
        <div class="left">
          <Hitokoto />
          <Music />
        </div>
      </el-col>
      <el-col :span="12">
        <div class="right cards">
          <div class="time">
            <div class="text">
              <span class="countdown">{{ formatCountdown(countdown) }}</span>
            </div>
          </div>
          <Weather />
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { ref, onMounted, onBeforeUnmount } from "vue";
import { mainStore } from "@/store";
import Music from "@/components/Music.vue";
import Hitokoto from "@/components/Hitokoto.vue";
import Weather from "@/components/Weather.vue";

const store = mainStore();

// Calculate countdown time
const currentTime = new Date();
const nextDay = new Date(currentTime.getFullYear(), currentTime.getMonth(), currentTime.getDate() + 1);
const countdown = ref(getTimeDifference(currentTime, nextDay));
const countdownInterval = ref(null);

// Update countdown time
const updateCountdown = () => {
  const now = new Date();
  countdown.value = getTimeDifference(now, nextDay);
};

onMounted(() => {
  updateCountdown();
  countdownInterval.value = setInterval(updateCountdown, 1000);
});

onBeforeUnmount(() => {
  clearInterval(countdownInterval.value);
});

// Format countdown time to HH:mm:ss
const formatCountdown = (time) => {
  const formattedHour = String(time.hours).padStart(2, '0');
  const formattedMinute = String(time.minutes).padStart(2, '0');
  const formattedSecond = String(time.seconds).padStart(2, '0');
  return `${formattedHour}:${formattedMinute}:${formattedSecond}`;
};

// Calculate time difference
const getTimeDifference = (start, end) => {
  const timeDifference = end - start;
  const hours = Math.floor((timeDifference / (1000 * 60 * 60)) % 24);
  const minutes = Math.floor((timeDifference / (1000 * 60)) % 60);
  const seconds = Math.floor((timeDifference / 1000) % 60);
  return { hours, minutes, seconds };
};
</script>

<style lang="scss" scoped>
// Your styles here
</style>

<style lang="scss" scoped>
.function {
  height: 165px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  &.mobile {
    .el-row {
      .el-col {
        &:nth-of-type(1) {
          display: contents;
        }
        &:nth-of-type(2) {
          display: none;
        }
      }
    }
  }
  .el-row {
    height: 100%;
    width: 100%;
    margin: 0 !important;
    .el-col {
      &:nth-of-type(1) {
        padding-left: 0 !important;
      }
      &:nth-of-type(2) {
        padding-right: 0 !important;
      }
      @media (max-width: 910px) {
        &:nth-of-type(1) {
          display: none;
        }
        &:nth-of-type(2) {
          padding: 0 !important;
          flex: none;
          max-width: none;
          width: 100%;
        }
      }
    }
    .left,
    .right {
      width: 100%;
      height: 100%;
    }
    .right {
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      animation: fade 0.5s;
      .time {
        font-size: 1.1rem;
        text-align: center;
        .date {
          text-overflow: ellipsis;
          overflow-x: hidden;
          white-space: nowrap;
        }
        .text {
          margin-top: 10px;
          font-size: 3.25rem;
          letter-spacing: 2px;
          font-family: "UnidreamLED";
        }
      }
      .weather {
        text-align: center;
        width: 100%;
        text-overflow: ellipsis;
        overflow-x: hidden;
        white-space: nowrap;
      }
    }
  }
}
</style>
