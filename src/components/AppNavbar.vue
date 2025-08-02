<script setup>
import { ref, onMounted } from 'vue'

const navItems = ref([])

onMounted(async () => {
    try {
        const response = await fetch('/api/get_navbar.json')
        const data = await response.json()
        navItems.value = data.data
    } catch (error) {
        console.error('获取导航数据失败:', error)
    }
})
</script>

<template>
    <div class="navbar bg-base-100 shadow-sm fixed top-0 left-0 right-0 z-50">
        <div class="navbar-start">
            <div class="dropdown">
                <div tabindex="0" role="button" class="btn btn-ghost lg:hidden">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24"
                        stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="M4 6h16M4 12h8m-8 6h16" />
                    </svg>
                </div>
                <ul tabindex="0" class="menu menu-sm dropdown-content bg-base-100 rounded-box z-1 mt-3 w-52 p-2 shadow">
                    <li v-for="item in navItems" :key="item.id">
                        <a :href="item.url" target="_blank">{{ item.title }}</a>
                    </li>
                </ul>
            </div>
            <RouterLink to="/" class="btn btn-ghost text-xl">GTA5中配</RouterLink>
        </div>
        <div class="navbar-center hidden lg:flex">
            <ul class="menu menu-horizontal px-1">
                <li v-for="item in navItems" :key="item.id">
                    <a :href="item.url" target="_blank">{{ item.title }}</a>
                </li>
            </ul>
        </div>
        <div class="navbar-end">
            <a class="btn" href="https://qm.qq.com/q/xbeh7ubNsc" target="_blank">加群</a>
        </div>
    </div>
</template>