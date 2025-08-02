<script setup>

const log = `
       　  　▃▆█▇▄▖
　 　 　 ▟◤▖　　　◥█▎
   　 ◢◤　 ▐　　　 　▐▉
　 ▗◤　　　▂　▗▖　　▕█▎
　◤　▗▅▖◥▄　▀◣　　█▊
▐　▕▎◥▖◣◤　　　　◢██
█◣　◥▅█▀　　　　▐██◤
▐█▙▂　　     　◢██◤
◥██◣　　　　◢▄◤
 　　▀██▅▇▀    

憋扒辣! 没有后端api全靠json模拟数据
欢迎灌♂注我和蝈牢师探♂讨♂学♂习一起van游♂戏
https://space.bilibili.com/37706580
https://space.bilibili.com/435502585
`

onMounted(() => {
  console.log(log)
})
</script>

<template>
  <header>
    <AppNavbar />
  </header>
  <main class="min-h-screen bg-base-200 pt-16">
    <RouterView />
  </main>
  <footer class="footer sm:footer-horizontal bg-neutral text-neutral-content p-10">
    <AppFooter />
  </footer>
</template>