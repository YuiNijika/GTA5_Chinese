<script setup>
import { ref, onMounted } from 'vue'

const characters = ref([])

onMounted(async () => {
    try {
        const response = await fetch('/api/get_character.json')
        const data = await response.json()
        characters.value = data.data.index
    } catch (error) {
        console.error('没有请求到数据:', error)
    }
})
</script>

<template>
    <div class="container mx-auto py-16 px-4">
        <h2 class="text-4xl font-bold text-center mb-12">主要角色</h2>
        <div class="grid grid-cols-1 lg:grid-cols-4 md:grid-cols-2 gap-8">
            <div v-for="character in characters" :key="character.id" class="card bg-base-100 shadow-xl">
                <figure>
                    <img :src="`/images/${character.image}`" :alt="character.name"
                        class="w-full h-64 object-cover" />
                </figure>
                <div class="card-body">
                    <h3 class="card-title">{{ character.name }}</h3>
                    <p>CV: {{ character.cv.name }}</p>
                </div>
            </div>
        </div>
    </div>
</template>