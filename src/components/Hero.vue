<script setup>
import {ref, onMounted} from 'vue'

const currentAudio = ref(null)
const isPlaying = ref(false)
const downloadData = ref([])
const origin_download_data = ref([])

const playAudio = () => {
  if (!currentAudio.value) {
    currentAudio.value = new Audio(`audio/bgm.m4a`)
    currentAudio.value.addEventListener('ended', onAudioEnded)
  }
  currentAudio.value.play()
  isPlaying.value = true
}

const onAudioEnded = () => {
  // 音频播放完毕时自动继续播放
  playAudio()
}

const toggleAudio = () => {
  if (isPlaying.value) {
    currentAudio.value.pause()
    isPlaying.value = false
  } else {
    playAudio()
  }
}

onMounted(async () => {
  if (!currentAudio.value) {
    // 模拟点击播放按钮
    // const btn = document.getElementById('bgm-btn')
    // btn.click()
  }
  try {
    const response = await fetch('/api/get_download.json')
    const data = await response.json()
    downloadData.value = data.data
    console.log('下载数据:', downloadData)
  } catch (error) {
    console.error('没有请求到数据:', error)
  }
})
</script>

<template>
  <div class="hero min-h-[70vh]"
       style="background-image: url('https://media-rockstargames-com.akamaized.net/tina-uploads/tina-modules/3871/07edb091bb329145ee3f3f01476adb40acc6457b.jpg');">
    <div class="hero-overlay bg-opacity-60"></div>
    <div class="hero-content text-center text-neutral-content">
      <div class="max-w-10xl flex justify-center items-center gap-10">
        <div class="max-w-10xl flex flex-col lg:flex-row justify-center items-center gap-10">
          <div class="w-full lg:w-1/2 flex flex-col items-center justify-center p-4">
            <h1 class="mb-5 text-3xl md:text-5xl font-bold">GTAV 国语配音</h1>
            <p class="mb-5 text-center">
              老一辈GTA艺术家倾情打造，好麦坞专业文案/配音团队匠心巨制，感受国语配音带来的沉浸式游戏体验吧！
            </p>
            <div class="flex gap-4">
              <button class="btn btn-primary btn-lg" onclick="v_dialog.showModal()">立即下载</button>
            </div>
          </div>
          <div class="w-full lg:w-1/2 flex justify-center p-4">
            <iframe
                src="//bilibili.com/blackboard/html5mobileplayer.html?bvid=BV1Kd5GzeEHb&autoplay=0&hideCoverInfo=0&danmaku=0"
                class="w-full max-w-full aspect-video"
                allowfullscreen
            ></iframe>
          </div>
        </div>

      </div>
    </div>
    <div class="fixed bottom-4 right-4 z-50">
      <button @click="toggleAudio" class="btn btn-circle btn-primary" id="bgm-btn">
        <svg v-if="isPlaying" xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
             stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M15.536 8.464a5 5 0 010 7.072M12 6a7.975 7.975 0 015.657 2.343m0 0a7.975 7.975 0 010 11.314m-11.314 0a7.975 7.975 0 010-11.314m0 0a7.975 7.975 0 015.657-2.333"/>
        </svg>
        <svg v-else xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24"
             stroke="currentColor">
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M5.586 15H4a1 1 0 01-1-1v-4a1 1 0 011-1h1.586l4.707-4.707C10.923 3.663 12 4.109 12 5v14c0 .891-1.077 1.337-1.707.707L5.586 15z"/>
          <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                d="M17 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2"/>
        </svg>
      </button>
    </div>
  </div>
  <dialog id="v_dialog" class="modal">
    <div class="modal-box w-11/12 max-w-5xl">
      <h3 class="text-lg font-bold">请选择下载方式</h3>
      <form method="dialog">
        <button class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2">✕</button>
      </form>
      <p class="py-4" v-html="downloadData.text"></p>
      <div class="modal-action">
        <form method="dialog">
          <a v-for="item in downloadData.download" :key="item.id" :href="item.url"
             class="btn btn-soft btn-secondary mr-2"
             target="_blank">
            {{ item.title }}
          </a>
        </form>
      </div>
    </div>
  </dialog>
</template>

