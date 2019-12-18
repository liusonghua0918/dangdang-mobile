<template>
  <div class="wrap">
    <div class="img">
      <img :src="detail.image"/>
    </div>
    <div class="inner">
      <div class="inner-head">
        <span class="self">当当自营</span>
        <span class="bookname">{{detail.title}}</span>
      </div>
      <div class="inner-inner">
          {{detail.desc}}
      </div>
      <div class="newprice">
        <span class="o">￥</span>
        <span class="price">{{detail.old_price}}</span>
        <span class="sale">(1.1折)</span>
        <span class="tell">降价通知</span>
      </div>
      <div class="oldprice">
          <span class="old">定价</span>
          <span class="pri">￥{{detail.price}}</span>
      </div>
    </div>
    <div class="bgc">
      <div class="shop">
        <div class="shop-left">
          <div class="store">
            <image src="/static/bottom/store.png"></image>
            <span>店铺</span>
          </div>
          <div class="cart" @click="tobuy">
            <image src="/static/bottom/cart.png"></image>
            <span>购物车</span>
          </div>
        </div>
        <div class="shop-right">
          <div class="go-buy" @click="tobuy">
            <p>立即购买</p>
          </div>
          <div class="go-cart" @click="addcart(detail._id)">
            <p>加入购物车</p>
        </div>
      </div>
    </div>
    </div>

  </div>
</template>
<script>
export default {
  data() {
    return {
      // book: [],
      detail:{}
    };
  },

  methods: {
    // 添加购物车
    addcart(id){
      console.log(id)
      //读取缓存信息
      let products=wx.getStorageSync("cart") || [];
      // 根据ID判断在购物车(缓存)中是否存在
      let index=products.findIndex((item)=>{
        return item.id==id
      })
      if(index!=-1){
        // 添加的商品存在的情况下，对应当前添加的商品的数量(count)进行累加
        products[index].count+=1
      }else{
        // 添加的商品不存在的情况下，把商品信息push到数组内
        this.detail.count=1; //添加后数组内的count默认为1
        this.detail.checked = true;
        // console.log(this.detail)
        products.push(this.detail)
      }
      // 加入缓存
      wx.setStorageSync("cart", products);
      wx.showToast({
        title: '添加成功',
        icon: 'success',
        duration: 400,
        mask: false,
        success: (result)=>{
          
        },
        fail: ()=>{},
        complete: ()=>{}
      });
    },
    tobuy(){
      // 跳转到购物车
      wx.switchTab({
        url: '/pages/cart/main'
      });
    }
  },

  created() {
    // wx.request({
    //   url: "http://127.0.0.1:8080/book.json",
    //   success:(res=>{
    //       this.book=res.data
    //       console.log(res)
    //   })
    // });
    
  },

  onLoad(e) {
    console.log(e);
    let id = e.id
    wx.request({
      url:"http://localhost:3001/detail",
      data: {id},
      header: {
        'content-type': 'application/json' 
      },
      success :(res)=> {
        console.log(res.data[0])
        this.detail = res.data[0]
      }
    })
  }
};
</script>
<style scoped>
.wrap{
  height: 100%;
}
.img {
  width: 100%;
  height: 360px;
  /* background-color: red; */
  border-bottom: 1px solid gray;
}
.img image {
  width: 100%;
  height: 100%;
}
.inner {
  /* height: 220px; */
  padding: 0 10px;
  /* background-color: skyblue; */
  background-color: white;
}
.inner-head {
  height: 50px;
  display: flex;
  align-items: center;
}
.self {
  background-color: red;
  border-radius: 14px;
  color: white;
  width: 60px;
  height: 24px;
  line-height: 24px;
  text-align: center;
  font-size: 14px;
}
.bookname {
  margin-left: 10px;
  font-size: 14px;
}
.inner-inner {
  width: 100%;
  /* height: 100%; */
  font-size: 14px;
  color: gray;
}
.newprice {
  margin-top: 10px;
  display: flex;
  align-items: center;
}
.o{
    color: red;
}
.price{
    color: red;
    font-size: 20px;
}
.sale{
    font-size: 14px;
    color: gray;
    margin-left: 4px;
    margin-right: 4px;
}
.tell{
    width: 80px;
    height: 20px;
    border: 1px solid black;
    border-radius: 14px;
    font-size: 14px;
    line-height: 20px;
    text-align: center;
}
.oldprice{
    margin-top: 10px;
    font-size: 14px;
    color: gray;
}
.shop{
  width: 100%;
  height: 60px;
  /* background-color: red; */
  position: fixed;
  bottom: 0;
  display: flex;
  justify-content: space-between;
}
.shop-left{
  width: 40%;
  display: flex;
  justify-content: space-around;
  background-color:white;
}
.shop-right{
  width: 60%;
  display: flex;
}
.store{
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 60px;
  justify-content: center;
}
.store image{
  width: 50%;
  height: 50%;
}
.cart{
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 60px;
}
.cart image{
  width: 50%;
  height: 50%;
}
.go-buy{
  width: 50%;
  height: 60px;
  line-height: 60px;
  text-align: center;
  background-color: #ffbe25;
  color: white;
}
.go-buy p{
  height: 100%;
}
.go-cart{
  width: 50%;
  height: 60px;
  line-height: 60px;
  text-align: center;
  background-color:#f3554a;
  color: white;
}
.go-cart p{
  height: 100%;
}
.bgc{
  height: 70px;
  background-color: #f8f8f8
}
</style>