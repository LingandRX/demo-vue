<script lang="js" setup>
import { onBeforeUnmount, ref } from 'vue';
import IconCangshu from './icons/IconCangshu.vue';
const progress = ref(0);
const playStatus = ref(false);

function handleClick(event) {
    console.log('click');
    const offsetX = event.offsetX;
    console.log(offsetX);

    progress.value = offsetX / 240 * 100;
    console.log(progress.value);
}

let intervalId;

async function handlePlay() {
    playStatus.value = !playStatus.value;

    if (progress.value >= 100 && playStatus.value) {
        progress.value = 0;
        console.log('reset');
    } else if (progress.value < 100 && !playStatus.value) {
        console.log('pause');
        clearInterval(intervalId);
    }

    if (playStatus.value) {
        intervalId = setInterval(() => {
            console.log('play...');
            progress.value += 20;
            if (progress.value >= 100) {
                console.log('end paly');
                playStatus.value = false;
                clearInterval(intervalId);
            }
        }, 1000);
    }
}

onBeforeUnmount(() => {
    if (intervalId) {
        clearInterval(intervalId);
    }
});
</script>

<template>
  <div class="play-container">
    <div @click="handlePlay"><IconCangshu /></div>

    <div class="progress-container" @click="handleClick">
      <div class="progress" :style="{ '--progress': progress + '%' }"></div>
    </div>
  </div>
</template>

<style scoped>
.play-container {
  display: flex;
  justify-content: center;
  min-width: 300px;
  max-width: 320px;
  margin: auto;
  padding: 10px 0;
  box-shadow: 0 0 5px 1px rgb(212, 227, 227, 0.5); /* 原始颜色为 #0ff */
}

@property --progress {
  syntax: '<percentage>';
  inherits: false;
  initial-value: 20%;
}

.progress-container {
  width: 240px;
  height: 10px;
  border-radius: 25px;
  background: #eee;
  margin: auto 10px;
}

.progress {
  width: --progress;
  height: inherit;
  border-radius: 25px 0 0 25px;
  background: linear-gradient(90deg, #0f0, #0ff var(--progress), transparent 0);
  transition: 0.3s --progress;
}
</style>
