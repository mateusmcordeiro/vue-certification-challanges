
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="timer.nowDays"  />
        <CountdownSegment data-test="hours" label="hours" :number="timer.nowHour"  />
        <CountdownSegment data-test="minutes" label="minutes" :number="timer.nowMinutes"  />
        <CountdownSegment data-test="seconds" label="seconds" :number="timer.nowSeconds" />
      </main>
    </div>
  </div>
</template>
<script setup>
import {computed,onUnmounted,ref} from 'vue';
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";

const nowDate = new Date().getTime();
const futureDate = new Date("2025-01-01T00:00:00").getTime();

const timeDiff = ref(futureDate - nowDate);

const interval = setInterval(() => {
  timeDiff.value = timeDiff.value - 1000;
},1000);

const timer = computed(() => {
  const nowTimeDiff = new Date(timeDiff.value);

  
  const [nowDays,nowHour, nowMinutes, nowSeconds] = [
  Math.ceil(timeDiff.value / (1000 * 3600 * 24)),
  nowTimeDiff.getHours(),
  nowTimeDiff.getMinutes(),
  nowTimeDiff.getSeconds(),

];

  return {
    nowDays,nowHour, nowMinutes, nowSeconds
  }
});



onUnmounted(() => {
  clearInterval(interval);
})



</script>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
