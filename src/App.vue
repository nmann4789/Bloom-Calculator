<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style lang="less">
  *{
    font-family: spinnaker,sans-serif;
  }
  body{
    background: url(https://static.wixstatic.com/media/11062b_616ea7fc12e64f158a528c7e04cb4fc9f000.jpg/v1/fill/w_1920,h_1080,al_c,q_85/11062b_616ea7fc12e64f158a528c7e04cb4fc9f000.webp);
    background-size: cover;
    background-repeat: no-repeat;
  }
  .center{
    text-align: center;
  }
</style>
