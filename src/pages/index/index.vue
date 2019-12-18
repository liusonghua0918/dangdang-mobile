<template>
    <div class="wrap">
      <div class="head">
        <div class="left">
          <img src="/static/img/head/logo.png">
        </div>
        <div class="ipt">
          <div class="pic">
            <img src="/static/img/head/search.png">
          </div>
          <input type="text">
        </div>
        <div class="right">
          <img src="/static/img/head/catalog.png" alt="">
        </div>
      </div>
      <swiper class="banners" indicator-dots indicator-color="purple" indicator-active-color="pink" interval="3000" duration="2000" autoplay current="0" circular >
          <swiper-item class="items">
            <image src="/static/img/banner/1.jpg"></image>
          </swiper-item>
          <swiper-item class="items">
            <image src="/static/img/banner/2.jpg"></image>
          </swiper-item>
          <swiper-item class="items">
            <image src="/static/img/banner/3.jpg"></image>
          </swiper-item>
          <swiper-item class="items">
            <image src="/static/img/banner/4.jpg"></image>
          </swiper-item>
          <swiper-item class="items">
            <image src="/static/img/banner/5.jpg"></image>
          </swiper-item>
      </swiper>
      <div class="classification">
        <ul>
          <li v-for="(item,index) in lists" :key="index">
            <img :src="item.image" />
          </li>
        </ul>
      </div>
      <div class="advertisement">
        <img src="/static/img/advertisement/1.jpg" alt="">
      </div>
      <div class="list">
        <ul>
          <!-- <navigator url="/pages/details/main" class="nav"> -->
            <li v-for="(item,index) in list" :key="index" @click="go(item._id)">
            <img :src="item.image">
            <p>{{item.title}}</p>
            <span class="self">当当自营</span>
            <span class="time">限时抢</span>
            <div style="margin-top:10px">
              <span class="price">￥{{item.price}}</span>
              <span class="price">￥{{item.old_price}}</span>
            </div>
          </li>
          <!-- </navigator> -->
        </ul>
      </div>
    </div>
</template>
<script>
export default {
  data(){
    return{
      lists:[],
      list:[]
    }
  },
   
  methods:{
      go(id){
        // console.log(id)
          wx.navigateTo({
           url: `/pages/details/main?id=${id}`
          })
      }
  },
   
  mounted(){
    // wx.request({
    //   url:"http://127.0.0.1:8080/type.json",
    //   success:(res=>{
    //       this.lists=res.data 
    //   })
    // })
    wx.request({
      url:"http://127.0.0.1:3001/classification",
      success:(res=>{
          this.lists=res.data 
            console.log(res)
      })
    })
    // wx.request({
    //   url:"http://127.0.0.1:8080/book.json",
    //   success:(res=>{
    //       this.arr=res.data
    //   })
    // })
    wx.request({
      url:"http://localhost:3001/list",
      success:(res=>{
          this.list=res.data
          console.log(res)
      })
    })
  }
}
</script>
<style scoped>
.wrap{
  background-color: #eff4fa;
}
.head{
  height: 40px;
  background-color:white;
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.left{
  width: 30px;
  height: 30px;
  margin-left: 10px;
}
.left image{
  width: 100%;
  height: 100%;
}
.ipt{
  width: 64%;
  height: 30px;
  border-radius: 20px;
  background-color: #e8ebf0;
  display: flex;
  align-items: center;
}
.ipt .pic{
  width: 26px;
  height: 20px;
  margin-left: 10px;
  margin-right: 10px;
}
.ipt .pic image{
  width: 100%;
  height: 100%;
}
.ipt input{
  width: 100%;
  height: 100%;
  text-indent: 2rem;
}
.right{
  width: 30px;
  height: 30px;
  margin-right: 10px;
}
.right image{
  width: 100%;
  height: 100%;
}
.banners{
  width: 100%;
  height: 160px;
}
.banners .items{
  width: 100%;
  height: 100%;
}
.banners .items image{
  width: 100%;
  height: 100%;
}

.classification{
  height: 160px;
}
.classification ul{
  width: 100%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.classification ul li{
  width: 20%;
  height: 50%;
}
.classification ul li image{
  width: 100%;
  height: 100%;
}

.advertisement{
  height: 100px;
}
.advertisement img{
  width: 100%;
  height: 100%;
}

.list{
}
.list ul{
  width: 100%;
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}
.list ul li{
  width: 44%;
  height: 260px;
  margin-top: 10px;
  font-size: 12px;
  background-color: white;
  padding: 0 10px;
  /* display: flex;
  justify-content: space-between;
  flex-direction: column; */
}

.list ul li p{
  /* display: inline-block; */
  margin-bottom: 4px;
  height: 30px;
}
.list ul li .baoyou{
  height: 16px;
  border: 1px solid red;
  color: red;
  margin-right: 4px;
}
.list ul li .self{
  height: 16px;
  border: 1px solid red;
  color: red;
  border-radius: 2px;
  font-size: 12px;
  margin-right: 4px;
}
.list ul li .time{
    height: 16px;
  background-color: red;
  color: white;
    font-size: 12px;
}
.list ul li .price{
  color: red;
  margin-top: 4px;
}
.list ul li img{
  width: 100%;
  height: 70%;
}
</style>