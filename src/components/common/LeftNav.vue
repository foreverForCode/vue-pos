<template>
  <div class="leftNav">
    <ul>
      <li>
        <i class="icon iconfont icon-wodezichan"></i>
        <div>收银</div>
      </li>
      <li>
        <i class="icon iconfont icon-dianpu"></i>
        <p>店铺</p>
      </li>
      <li>
        <i class="icon iconfont icon-hanbao"></i>
        <p>汉堡</p>
      </li>
      <li>
        <i class="icon iconfont icon-huiyuanqia"></i>
        <p>会员</p>
      </li>
      <li>
        <i class="icon iconfont icon-zuanshi"></i>
        <p>统计</p>
      </li>
      <li>
        <i class="icon iconfont icon-wodezichan"></i>
        <p>收银</p>
      </li>
      <li>
        <i class="icon iconfont icon-gongnengjianyi"></i>
        <p>设置</p>
      </li>
    </ul>
  </div>
</template>
<script>
  export default{
    
    data(){
      return {

      }
    }
  }
</script>

<style>
  .leftNav{
    color:#fff;
    font-size:10px;
    height:100%;
    background-color:#1d8ce0;
    float:left;
    width:60px;
  }
  .iconfont{
    font-size:24px;
  }
  .leftNav ul{
    margin:0;
    padding:0
  }
  .leftNav ul li{ 
    list-style:none;
    text-align:center;
    border-bottom:1px solid #20a0ff;
    padding:10px;
  }
</style>