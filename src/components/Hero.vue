<script setup>
import { ref, onMounted } from 'vue'

const downloadUrl = ref([])

onMounted(async () => {
    try {
        const response = await fetch('/api/get_download.json')
        const data = await response.json()
        downloadUrl.value = data.data
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
            <div class="max-w-md">
                <h1 class="mb-5 text-5xl font-bold">GTA5 中文配音</h1>
                <p class="mb-5">体验洛圣都的全新声音，感受中文配音带来的沉浸式游戏体验</p>
                <button class="btn btn-primary" onclick="my_modal_5.showModal()">立即下载</button>
            </div>
        </div>
    </div>
    <dialog id="my_modal_5" class="modal">
        <div class="modal-box w-11/12 max-w-5xl">
            <h3 class="text-lg font-bold">感谢下载!</h3>
            <form method="dialog">
                <button class="btn btn-sm btn-circle btn-ghost absolute right-2 top-2">✕</button>
            </form>
            <p class="py-4">推荐使用直链下载, 如果下载失败请加群 <a href="https://qm.qq.com/q/xbeh7ubNsc" target="_blank">939762012</a> 讨论和交流</p>
            <div class="modal-action">
                <form method="dialog">
                    <a v-for="item in downloadUrl" :key="item.id" :href="item.url" class="btn btn-soft btn-secondary mr-2"
                        target="_blank">
                        {{ item.title }}
                    </a>
                    <a href="https://gta5-chinese-r2.x-x.work/GTA5%E4%B8%AD%E9%85%8DMOD%E8%AF%B4%E6%98%8E%E4%B9%A6.pdf" target="_blank" class="btn btn-soft btn-warning">说明书</a>
                </form>
            </div>
        </div>
    </dialog>
</template>