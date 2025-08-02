<script setup>
import {ref, onMounted} from 'vue'
import MarkdownIt from 'markdown-it'
import 'github-markdown-css';

const htmlContent = ref('<p>加载中...</p>')
const md = new MarkdownIt();

onMounted(async () => {
  try {
    const response = await fetch('/api/manual.md')
    let text = await response.text();
    htmlContent.value = md.render(text);
  } catch (error) {
    console.error('加载失败:', error)
    htmlContent.value = `
        <span>说明书加载失败: ${error.message}</span>
    `
  }
})
</script>

<template>
  <input type="radio" name="my_tabs_2" class="tab" aria-label="说明"/>

  <div class="tab-content markdown-body border-base-300 bg-base-100 p-10" v-html="htmlContent">
  </div>
</template>
