<script setup lang="ts">
import { onMounted, reactive, ref, defineProps } from "vue";

interface DateTime {
  date: string;
  time: string;
}

const props = defineProps({
  isPlaying: Boolean,
});
const isPlaying = ref(props.isPlaying);

const dateTime = reactive<DateTime>({
  date: "",
  time: "",
});

/**
 * 現在の日時を更新
 */
const updateDateTime = () => {
  let currentDate = new Date();
  const year = zeroPadding(currentDate.getFullYear(), 4);
  const month = zeroPadding(currentDate.getMonth() + 1, 2);
  const day = zeroPadding(currentDate.getDate(), 2);
  const weekList = ["(日)", "(月)", "(火)", "(水)", "(木)", "(金)", "(土)"];
  const week = weekList[currentDate.getDay()];
  const hours = zeroPadding(currentDate.getHours(), 2);
  const minutes = zeroPadding(currentDate.getMinutes(), 2);
  const seconds = zeroPadding(currentDate.getSeconds(), 2);

  dateTime.time = `${hours}:${minutes}:${seconds}`;
  dateTime.date = `${year}年${month}月${day}日${week}`;
};

onMounted(() => {
  setInterval(updateDateTime, 1000);
});

/**
 * ゼロパディング
 *
 * @param num 数値
 * @param length 桁数
 */
const zeroPadding = (num: number, length: number) => {
  return String(num)
    .padStart(length, "0")
    .slice(-1 * length);
};

const displayHandler = () => {
  isPlaying.value = true;
};
</script>

<style scoped>
p {
  text-align: center;
  color: rgb(232, 153, 153);
}
</style>

<template>
  <button v-if="!isPlaying" @click="displayHandler">ここをクリック</button>
  <div v-else class="wrap">
    <h2>現在の日時は</h2>
    <p class="date">{{ dateTime.date }}</p>
    <p class="time">{{ dateTime.time }}</p>
  </div>
</template>
