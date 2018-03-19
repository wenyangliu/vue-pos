<template>
    <div class="pos">
       <el-row>
           <el-col :span='7' class="pos-order" id="order-list">
               <el-tabs>
                   <el-tab-pane label="点餐">
                       <el-table :data="tableData" border show-summary style="width: 100%;">
                           <el-table-column prop="goodsName" label="商品"></el-table-column>
                           <el-table-column prop="count" label="量" width="50"></el-table-column>
                           <el-table-column prop="price" label="金额" width="70"></el-table-column>
                           <el-table-column label="操作" width="100" fixed="right">
                               <template slot-scope="scope">
                                   <el-button type="text" size="small">删除</el-button>
                                   <el-button type="text" size="small">增加</el-button>
                               </template>
                           </el-table-column>
                       </el-table>

                       <div class="order-btn">
                           <el-button type="warning">挂单</el-button>
                           <el-button type="danger">删除</el-button>
                           <el-button type="success">结账</el-button>
                       </div>
                   </el-tab-pane>

                   <el-tab-pane label="挂单">
                       挂单
                   </el-tab-pane>

                   <el-tab-pane label="外卖">
                       外卖
                   </el-tab-pane>

               </el-tabs>
           </el-col>
           <el-col :span='17'>

               <div class="often-goods">
                   <div class="title">常用商品</div>
                   <div class="often-goods-list">
                       <ul>
                           <li v-for="goods in oftenGoods">
                               <span>{{goods.goodsName}}</span>
                               <span class="o-price">￥{{goods.price}}元</span>
                           </li>
                       </ul>
                   </div>
               </div>

               <div class="goods-type">
                   <el-tabs>

                       <el-tab-pane label="汉堡">
                           <ul class="cookList">
                               <li v-for="goods in type0Goods">
                                   <span class="foodImg">
                                       <img :src="goods.goodsImg" width="100%">
                                   </span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>

                       <el-tab-pane label="小食">
                           <ul class="cookList">
                               <li v-for="goods in type1Goods">
                                   <span class="foodImg">
                                       <img :src="goods.goodsImg" width="100%">
                                   </span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>

                       <el-tab-pane label="饮料">
                           <ul class="cookList">
                               <li v-for="goods in type2Goods">
                                   <span class="foodImg">
                                       <img :src="goods.goodsImg" width="100%">
                                   </span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">￥{{goods.price}}元</span>
                               </li>
                           </ul>
                       </el-tab-pane>

                       <el-tab-pane label="套餐">
                           <ul class="cookList">
                               <li v-for="goods in type3Goods">
                                   <span class="foodImg">
                                       <img :src="goods.goodsImg" width="100%">
                                   </span>
                                   <span class="foodName">{{goods.goodsName}}</span>
                                   <span class="foodPrice">￥{{goods.price}}元</span>
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
    import axios from 'axios'
    export default {
        name: 'Pos',
        created() {
          //读取常用商品列表
          axios.get('http://jspang.com/DemoApi/oftenGoods.php')
            .then(res => {
              console.log(res)
              this.oftenGoods = res.data
            })
            .catch(error => {
              console.log(error)
              alert('网络错误，不能访问')
            })
          //读取分类商品列表
          axios.get('http://jspang.com/DemoApi/typeGoods.php')
            .then(res => {
              this.type0Goods = res.data[0]
              this.type1Goods = res.data[1]
              this.type2Goods = res.data[2]
              this.type3Goods = res.data[3]
            })
            .catch(error => {
              console.log(error)
              alert('网络错误，不能访问')
            })
        },
      mounted: function () {
        let orderHeight = document.body.clientHeight
        document.getElementById('order-list').style.height = orderHeight + 'px'
      },
        data () {
            return {
                tableData: [{
                  goodsName: '可口可乐',
                  price: 8,
                  count:1
                }, {
                  goodsName: '香辣鸡腿堡',
                  price: 15,
                  count:1
                }, {goodsName: '爱心薯条',price: 8, count:1}, {
                  goodsName: '甜筒',
                  price: 8,
                  count:1
                }],
              oftenGoods: [],
              type0Goods: [],
              type1Goods: [],
              type2Goods: [],
              type3Goods: [],
              totalMoney: 0, //订单总价格
              totalCount: 0  //订单商品总数量
            }
        },

    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .pos{
        font-size: 12px;
    }
    .pos-order{

        background-color: #F9FAFC;
        border-right:1px solid #C0CCDA;
    }
    .order-btn{
        margin-top:10px;
        text-align: center;
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
    .goods-type{
        clear:both;
    }
    .o-price{
        color:#58B7FF;
    }
    .often-goods-list{
        border-bottom:1px solid #C0CCDA;
        height:auto;
        overflow: hidden;
        padding-bottom:10px;
        background-color:#F9FAFC;
    }
    .cookList li{
        list-style: none;
        width:23%;
        border:1px solid #E5E9F2;
        height: auto;
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
    }
    .foodPrice{
        font-size: 16px;
        padding-left: 10px;
        padding-top:10px;
    }
</style>
