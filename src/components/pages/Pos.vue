<template>
  <div class="pos">  
    <el-row>  
      <el-col :span='7' class="orderList" id="order-list">  
        <el-tabs v-model="activeName">  
          <el-tab-pane label="点餐" name="first">
            <el-table border style="width:100%" :data='orderData'>
              <el-table-column prop="goodsName" label='商品名称'>
              
              </el-table-column>
              <el-table-column prop="count" label='数量' width='100'>
              
              </el-table-column>
              <el-table-column prop="price" label='价格' width='100'>
              </el-table-column>
              <el-table-column fixed= 'right' prop="operation" label='操作' width='100'>
                <template scope='scope'>
                  <el-button type='text' size='small' @click="delSingleProduce(scope.row)">删除</el-button>
                  <el-button type='text' size='small' @click="addProduct(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
          <small>总数：</small> {{totalMount}}
          <small>总价：</small> {{totalPrice}}
        </div>
          </el-tab-pane>
  
          <el-tab-pane label="挂单" name="second" >挂单</el-tab-pane>
  
          <el-tab-pane label="外卖" name="third">外卖</el-tab-pane>
  
  
        </el-tabs>
        
        <div class="btn">
          <el-button type='success' size='small' @click="checkout">结账</el-button>
          <el-button type='warning' size='small'>挂单</el-button>
          <el-button type='danger' size='small' @click="delAllProduct">删除</el-button>
        </div>
  
      </el-col>
  
  
  
      <el-col :span='17'>
        <div class="often-goods">
          <div class="title">
            常用商品
          </div>
          <div class="often-goods-list">
            <ul>
              <li v-for="good in oftenGoods" @click="addProduct(good)">
                <span>{{good.goodsName}}</span>
                <span class="o-price">￥{{good.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goods-type">
 
    <el-tabs>
        <el-tab-pane label="汉堡">
            <ul class='cookList'>
              <li v-for="good in type0Goods" @click="addProduct(good)">
                  <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                  <span class="foodName">{{good.goodsName}}</span>
                  <span class="foodPrice">￥{{good.price}}元</span>
              </li>
          </ul>
        </el-tab-pane>
            <el-tab-pane label="小食">
            <ul class='cookList'>
              <li v-for="good in type1Goods" @click="addProduct(good)">
                  <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                  <span class="foodName">{{good.goodsName}}</span>
                  <span class="foodPrice">￥{{good.price}}元</span>
              </li>
          </ul>
        </el-tab-pane>
        <el-tab-pane label="饮料">
            <ul class='cookList'>
              <li v-for="good in type2Goods" @click="addProduct(good)">
                  <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                  <span class="foodName">{{good.goodsName}}</span>
                  <span class="foodPrice">￥{{good.price}}元</span>
              </li>
          </ul>
        </el-tab-pane>
        <el-tab-pane label="套餐">
            <ul class='cookList'>
              <li v-for="good in type3Goods" @click="addProduct(good)">
                  <span class="foodImg"><img :src="good.goodsImg" width="100%"></span>
                  <span class="foodName">{{good.goodsName}}</span>
                  <span class="foodPrice">￥{{good.price}}元</span>
              </li>
          </ul>
        </el-tab-pane>
 
    </el-tabs>
</div>       
      </el-col>
  
    </el-row>
  
  
  
  </div>
</template>

<script>
  import axios from'axios';

  export default {
    
    name: 'pos',  
    data() {  
      return {  
        orderHeight: 0,
        orderData:[],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        totalMount:0,
        totalPrice:0,
        activeName:'first'
      }  
    },
    methods:{

      // 添加订单到订单列表
      addProduct(product){
         
        

        // 判断 订单列表是否有该项商品 如果有 加数量 没有 加商品

        let isHave = false;

        for(var i = 0;i<this.orderData.length;i++){
          if(this.orderData[i].goodsId == product.goodsId){
            isHave = true;
          }
        };

        // 判断isHave 是否为真，如果是真 那么 数量加 1 否则 添加 新的产品

        if(isHave){
          let arr = this.orderData.filter(o =>o.goodsId == product.goodsId);
          arr[0].count++
        }else{
          //不存在就推入数组
          let newGoods={goodsId:product.goodsId,goodsName:product.goodsName,price:product.price,count:1};
          this.orderData.push(newGoods);
        }
        this.collectd();
        

        
      },
      // 把汇总功能提取成一个模块
      collectd(){
      // 总数量 和 总价格 清零

        this.totalMount = 0;
        this.totalPrice = 0;

        // 进行 数量 和价格 汇总计算

        this.orderData.forEach((ele) =>{
          this.totalMount += ele.count;
          this.totalPrice += (ele.price * ele.count)
        })

    },
    // 删除单个商品
    delSingleProduce(product){
      
       this.orderData = this.orderData.filter(item => item.goodsId !== product.goodsId);
       this.collectd();
    },

    // 删除所有商品
    delAllProduct(){

      this.orderData = [];
      this.totalMount = 0;
      this.totalPrice = 0;
    },

    // 结账
    checkout(){
      if(this.totalMount !== 0){
        this.delAllProduct();
        this.$message({
          message:'结账成功，感谢你的用餐',
          type:'success'
        })
      }else{
        this.$message.error('不能空结账')
      }
    }
    

      
    },
    
  
    created(){
      
      // URL 
      let oftenGoodsURL = 'http://jspang.com/DemoApi/oftenGoods.php';
      let typeGoodsURL = 'http://jspang.com/DemoApi/typeGoods.php';

      // 获取常用商品的数据
      axios.get(oftenGoodsURL).then(response =>{
        if(response.status === 200){
          this.oftenGoods = response.data;
        }else{
          this.oftenGoods = []
        }
      }).catch(error =>{
        console.log(error);
      })
      // 获取商品分类数据
      axios.get(typeGoodsURL).then(
        response =>{
          if(response.status === 200){
            this.type0Goods=response.data[0];
            this.type1Goods=response.data[1];
            this.type2Goods=response.data[2];
            this.type3Goods=response.data[3];
          }else{
            this.type0Goods=[];
            this.type1Goods=[];
            this.type2Goods=[];
            this.type3Goods=[];
          }
        }
      ).catch()
    },
    mounted() {
  
      this.orderHeight = document.body.clientHeight; // 获取屏幕的高度  
  
      document.getElementById('order-list').style.height = this.orderHeight + 'px'
  
    },
  
  
  
    watch: {
  
      orderHeight: function(newValue, oldValue) {
  
        document.getElementById('order-list').style.height = newValue + 'px'
  
      }
  
  
  
    }
  
  
  
  
  
  
  
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

  .orderList {
  
    background-color: #f9fafc;
    border-right:1px solid #c0ccda;
  
  }
  .btn{
    margin-top:20px;
    text-align:center
  }
  .title{
       height: 20px;
       border-bottom:1px solid #D3DCE6;
       background-color: #F9FAFC;
       padding:10px;
   }
   .often-goods-list ul li{
      list-style: none;
      float:left;
      border:1px solid #E5E9F2;
      padding:10px;
      margin:5px;
      background-color:#fff;
   }
  .o-price{
      color:#58B7FF; 
   }
   .goods-type{
     clear:both
   }
   .cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;
 
   }
   .cookList li span{
       
        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;
       width: 124px;
 
   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
  
</style>
