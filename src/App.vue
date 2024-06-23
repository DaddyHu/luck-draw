<template>
  <div class="container">
    <div class="swiper">
      <t-swiper
        v-model:current="current"
        :autoplay="false"
        :height="200"
        :navigation="{
          placement: 'outside',
          showSlideBtn: 'never',
          type: 'fraction',
        }"
        type="card"
      >
        <t-swiper-item v-for="_ in range" :key="_">
          <div class="box">
            {{ _ }}
          </div>
        </t-swiper-item>
      </t-swiper>
    </div>
    <t-divider style="width: 500px" />
    <t-space direction="vertical">
      <t-input-number
        v-model:value="min"
        :allowInputOverLimit="false"
        :decimalPlaces="0"
        label="最小值"
        :min="1"
        theme="column"
        style="width: 200px"
      />
      <t-input-number
        v-model:value="max"
        :allowInputOverLimit="false"
        :decimalPlaces="0"
        label="最大值"
        :min="1"
        theme="column"
        style="width: 200px"
      />
      <t-button block :loading="loading" @click="handleStart">开 始</t-button>
    </t-space>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const current = ref(0);
const min = ref(1);
const max = ref(6);
const range = computed(() => {
  const arr = [];
  for (let i = min.value; i <= max.value; i++) {
    arr.push(i);
  }
  return arr;
});
const loading = ref(false);
const totalTime = 1000;
const handleStart = () => {
  loading.value = true;
  const result =
    Math.floor(Math.random() * (max.value - min.value + 1)) + min.value;
  const interval = range.value.length + result;
  for (let i = 0; i < interval; i++) {
    setTimeout(() => {
      current.value = i % range.value.length;
      setTimeout(() => {
        if (i === interval - 1) loading.value = false;
      }, 300);
    }, (totalTime / interval) * i);
  }
};
</script>

<style>
* {
  box-sizing: border-box;
}

html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}
</style>

<style scoped>
.container {
  width: 100vw;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #ffffff;

  .swiper {
    width: 490px;

    .box {
      width: 200px;
      height: 200px;
      background-color: #eeeeee;
      display: flex;
      justify-content: center;
      align-items: center;
      font-size: 50px;
      font-weight: bold;
    }
    :deep(.t-swiper__navigation) {
      display: none;
    }
  }
}
</style>
