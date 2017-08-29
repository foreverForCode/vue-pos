<template>
  <div id="app">
  
    <LeftNav></LeftNav>

    <div class="main">
      <router-view></router-view>
    </div>
  
  </div>
</template>

<script>
  import LeftNav from '@/components/common/LeftNav';
  
  
  export default {
    name: 'app',
    data() {
      return {
  
      }
    },
    components: {
      LeftNav
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  .main{
    width:calc(100% - 60px);
    height:100%;
    float:left;
    overflow:hidden;
    background-color:#eff2f7
  }
</style>
