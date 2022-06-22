<template>
  <div class="container">
    <nav class="border m-2 p-3">
      <router-link to="/" class="fs-3">首頁</router-link>
      <router-link to="/date" class="fs-3 ms-5">日期練習</router-link>
      <router-link to="/upload" class="fs-3 ms-5">檔案上傳練習</router-link>
      <router-link to="/mix" class="fs-3 ms-5">表單驗證</router-link>
    </nav>
    <router-view />
  </div>
</template>

<style lang="scss">
@import "@/assets/stylesheets/all.scss";
</style>
