<script setup>
import { ref, onMounted } from 'vue'
import { marked } from 'marked'

const htmlContent = ref('<p>加载中...</p>')

onMounted(async () => {
    try {
        const response = await fetch('/api/manual.md')

        if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`)

        const markdownText = await response.text()
        htmlContent.value = marked.parse(markdownText, {
            breaks: true,
            gfm: true
        })
    } catch (error) {
        console.error('加载失败:', error)
        htmlContent.value = `
        <span>说明书加载失败: ${error.message}</span>
    `
    }
})
</script>

<template>
    <input type="radio" name="my_tabs_2" class="tab" aria-label="中配说明" />
    <div class="tab-content border-base-300 bg-base-100 p-10" v-html="htmlContent"></div>
</template>
