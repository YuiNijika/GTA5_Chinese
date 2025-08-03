<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const characters = ref([])
const currentAudio = ref(null)
const isLoading = ref(false)
const currentPlayingId = ref(null)

onMounted(async () => {
    try {
        const response = await fetch('/api/get_character.json')
        const data = await response.json()
        characters.value = data.data.all
    } catch (error) {
        console.error('没有请求到数据:', error)
    }
})

onUnmounted(() => {
    // 组件卸载时停止音频播放
    if (currentAudio.value) {
        currentAudio.value.pause()
        currentAudio.value = null
    }
})

const toggleAudio = (character) => {
    // 如果点击的是当前正在播放的音频
    if (currentPlayingId.value === character.id) {
        stopAudio()
        return
    }
    
    // 停止当前播放的音频
    if (currentAudio.value) {
        currentAudio.value.pause()
        currentAudio.value.currentTime = 0
        currentAudio.value.removeEventListener('ended', onAudioEnded)
    }

    // 设置加载状态和当前播放项
    isLoading.value = true
    currentPlayingId.value = character.id

    // 创建新的音频对象并播放
    currentAudio.value = new Audio(`audio/${character.cv.audio}`)

    // 监听加载完成事件
    currentAudio.value.addEventListener('canplaythrough', () => {
        isLoading.value = false
    })

    // 监听播放完成事件
    currentAudio.value.addEventListener('ended', onAudioEnded)

    // 监听播放错误
    currentAudio.value.addEventListener('error', () => {
        isLoading.value = false
        currentPlayingId.value = null
    })

    currentAudio.value.play().catch(() => {
        isLoading.value = false
        currentPlayingId.value = null
    })
}

const onAudioEnded = () => {
    // 音频播放完毕时重置状态
    currentAudio.value = null
    currentPlayingId.value = null
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
    currentPlayingId.value = null
}
</script>

<template>
    <input type="radio" name="my_tabs_2" class="tab" aria-label="主要角色CV" checked="checked" />
    <div class="tab-content mt-4">
        <div class="grid grid-cols-1 lg:grid-cols-4 md:grid-cols-2 gap-8">
            <div v-for="character in characters" :key="character.id" class="card bg-base-100 shadow-xl">
                <figure>
                    <img :src="`/images/${character.image}`" :alt="character.name" class="w-full h-64 object-cover" />
                </figure>
                <div class="card-body">
                    <h3 class="card-title">{{ character.name }}</h3>
                    <p>CV: {{ character.cv.name }}</p>
                    <div class="card-actions justify-end mt-2">
                        <button
                            @click="toggleAudio(character)"
                            class="btn btn-primary"
                            :class="{ 'btn-secondary': currentPlayingId === character.id && !isLoading }"
                        >
                            <span v-if="isLoading && currentPlayingId === character.id" class="loading loading-spinner"></span>
                            <span v-else-if="currentPlayingId === character.id">停止播放</span>
                            <span v-else>试听</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>