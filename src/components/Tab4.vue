<script setup>
import { ref, onMounted } from 'vue'

const demos = ref([])
const baseUrl = 'https://gta5-chinese-r2.x-x.work/web/audio'
const currentAudio = ref(null)
const isLoading = ref(false)
const currentPlaying = ref(null)

onMounted(async () => {
    try {
        const response = await fetch('/api/get_demo.json')
        const data = await response.json()
        demos.value = data.data
    } catch (error) {
        console.error('没有请求到数据:', error)
    }
})

const playAudio = (filename) => {
    // 停止当前播放的音频
    if (currentAudio.value) {
        currentAudio.value.pause()
        currentAudio.value.currentTime = 0
        currentAudio.value.removeEventListener('ended', onAudioEnded)
    }

    // 设置加载状态和当前播放项
    isLoading.value = true
    currentPlaying.value = filename

    // 创建新的音频对象并播放
    currentAudio.value = new Audio(`${baseUrl}/${filename}`)

    // 监听加载完成事件
    currentAudio.value.addEventListener('canplaythrough', () => {
        isLoading.value = false
    })

    // 监听播放完成事件
    currentAudio.value.addEventListener('ended', onAudioEnded)

    // 监听播放错误
    currentAudio.value.addEventListener('error', () => {
        isLoading.value = false
        currentPlaying.value = null
    })

    currentAudio.value.play().catch(() => {
        isLoading.value = false
        currentPlaying.value = null
    })
}

const onAudioEnded = () => {
    // 音频播放完毕时重置状态
    currentAudio.value = null
    currentPlaying.value = null
    isLoading.value = false
}

const stopAudio = () => {
    if (currentAudio.value) {
        currentAudio.value.pause()
        currentAudio.value.currentTime = 0
        currentAudio.value.removeEventListener('ended', onAudioEnded)
        currentAudio.value = null
    }
    isLoading.value = false
    currentPlaying.value = null
}
</script>

<template>
    <input type="radio" name="my_tabs_2" class="tab" aria-label="配音试听" />
    <div class="tab-content border-base-300 bg-base-100 p-10">
        <div class="mb-6 flex justify-center">
            <button v-if="currentPlaying && !isLoading" @click="stopAudio" class="btn btn-secondary">
                停止播放
            </button>
            <button v-else-if="currentPlaying && isLoading" class="btn btn-secondary" disabled>
                <span class="loading loading-spinner"></span>
            </button>
        </div>

        <div class="space-y-2">
            <div v-for="(demo, index) in demos" :key="index"
                class="flex items-center justify-between p-3 bg-base-200 rounded-lg hover:bg-base-300 transition-colors">
                <span class="text-lg">{{ demo }}</span>
                <div class="space-x-2">
                    <button @click="playAudio(demo)" class="btn btn-sm btn-primary"
                        :disabled="isLoading && currentPlaying === demo">
                        <span v-if="isLoading && currentPlaying === demo"
                            class="loading loading-spinner loading-xs"></span>
                        <span v-else>播放</span>
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>