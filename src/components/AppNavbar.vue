<script setup>
import { ref } from 'vue'

const navItems = ref([
    {
        title: '哔哩哔哩',
        url: 'https://space.bilibili.com/37706580'
    },
    {
        title: '爱发电',
        url: 'https://ifdian.net/a/Katock'
    }
])
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
                    <li v-for="(item, index) in navItems" :key="index">
                        <a :href="item.url" target="_blank">{{ item.title }}</a>
                    </li>
                </ul>
            </div>
            <a class="btn btn-ghost text-xl">GTA5中配</a>
        </div>
        <div class="navbar-center hidden lg:flex">
            <ul class="menu menu-horizontal px-1">
                <li v-for="(item, index) in navItems" :key="index">
                    <a :href="item.url" target="_blank">{{ item.title }}</a>
                </li>
            </ul>
        </div>
        <div class="navbar-end">
            <a class="btn" href="https://qm.qq.com/q/xbeh7ubNsc" target="_blank">加群</a>
        </div>
    </div>
</template>
