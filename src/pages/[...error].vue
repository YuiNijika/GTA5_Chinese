<script setup lang="ts">
import { RouterLink } from 'vue-router';

</script>

<template>
    <div class="hero bg-base-200 min-h-screen">
        <div class="hero-content text-center">
            <div class="max-w-md">
                <h1 class="text-5xl font-bold">error</h1>
                <p class="py-6">
                    这里什么都没有捏
                </p>
                <RouterLink to="/" class="btn btn-primary">返回首页</RouterLink>
            </div>
        </div>
    </div>
</template>