<template>
    <div class="wrap">
        <ul>
            <li v-for="(item,index) in products" :key="index">
                <p class="del" @click="del(index)">
                    <image src="/static/cart/del.png"></image>
                </p>
                <div class="li-top">
                    <div class="check">
                        <input type="checkbox" :checked="item.checked" @click="singleCheck(index)">
                    </div>
                    <div class="img"  @click="back(index)">
                        <image :src="item.image"></image>
                    </div>
                    <div class="right">
                        <p>{{item.title}}</p>
                        <div>
                            <span class="new">￥{{item.price}}</span>
                            <span class="old">￥{{item.old_price}}</span>
                            <span class="jian" @click="reduce(index)">-</span>
                            <span>{{item.count}}</span>
                            <span class="jia" @click="add(index)">+</span>
                        </div>
                    </div>
                </div>
                <div class="li-bottom">
                    <div class="li-bottom-left">
                        <span class="addprice">加价购</span>
                        <span class="other">购买1件，即可享受换购优惠</span>
                    </div>
                    <div>
                        <span>去选择></span>
                    </div>
                </div>
            </li>
        </ul>
        <div class="allprice">
            <div>
                <input type="checkbox" @click="allcheck" :checked="isCheckAll">
                <span>全选</span>
            </div>
            <div>
                <span>合计：</span>
                <span class="total">￥{{total}}</span>
            </div>
            <span class="settlement">结算
                <span>({{acount}})</span>
            </span>
        </div>
    </div>
</template>
<script>
export default {
    data(){
        return{
            products:[],  //存储一个变量为空数组
            total:0,  //默认总价为0
            acount:0,    // 默认结算的数量为0
            isCheckAll:true //默认全选中
        }
    },
     
    methods:{
        back(index){
            console.log(index)
            wx.navigateTo({
                // url: '/pages/details/main',
                success: (result)=>{
                    console.log("返回成功")
                },
                fail: ()=>{
                    console.log("返回失败")
                },
                complete: ()=>{}
            });
        },
        //添加
        add(index){  
            this.products[index].count+=1
            // 加入缓存
            wx.setStorageSync("cart",this.products);
            this.getTotal();
        },
        //减少
        reduce(index){ 
            this.products[index].count-=1
            if(this.products[index].count<1){
                this.products[index].count=1
            }
            // 加入缓存
            wx.setStorageSync("cart", this.products);
            this.getTotal();
        },
        // 删除
        del(index){
            console.log(index)
            this.products.splice(index,1)
            wx.setStorageSync("cart", this.products);
        },
        // 全选
        allcheck(){
            this.isCheckAll=!this.isCheckAll
            this.products.forEach(item=>{
                item.checked=this.isCheckAll
            })
            wx.setStorageSync("cart",this.products)
            this.getTotal()
        },
        // 总价
        getTotal(){
            let products=wx.getStorageSync("cart") || []
            // 默认为0
            let total=0
            let acount=0
            products.forEach((item) => {
                if(item.checked==true){
                    // 总价
                    total+=item.price*item.count
                    // 总数量
                    acount+=item.count
                }
            });
            // 渲染到页面内
            this.total=total
            this.acount=acount
            console.log(total)
        },
        //单选按钮
        singleCheck(index){
            // 状态相反
            this.products[index].checked=!this.products[index].checked
            // 加入缓存
            wx.setStorageSync("cart",this.products)
            // 判断全选
            this.isCheckAll=this.products.every(item=>{
                return item.checked==true
            })
            // 进行计算总金额
            this.getTotal()
        }
    },
     
    created(){
        this.getTotal()
    },
    onShow(){
        let products = wx.getStorageSync("cart");
        this.products=products
        console.log(products)
        this.getTotal()
    }
}
</script>
<style scoped>
.wrap{
    background-color: #f2f2f2;
}
.wrap ul{
    display: flex;
    padding:10px;
    flex-wrap: wrap;
    padding-bottom: 60px;
}
.wrap ul li{
    width: 100%;
    height: 200px;
    border-radius: 10px;
    display: flex;
    justify-content: space-between;
    flex-direction: column;
    font-size: 14px;
    background-color: white;
    margin-bottom: 10px;
    position: relative;
}
.wrap ul li .del{
    width: 20px;
    height: 20px;
    position: absolute;
    right: 20px;
    top: 20px;
}
.wrap ul li .del image{
    width: 100%;
    height: 100%;
}
.li-top{
    width: 100%;
    height: 70%;
    /* background-color: aqua; */
    display: flex;
    /* justify-content: space-between; */
    align-items: center;
}
.li-bottom{
    width: 100%;
    height: 30%;
    /* background-color: red; */
}
.img{
    width: 30px;
    height: 40px;
    background-color:skyblue;
}
.img image{
    width: 100%;
    height: 100%;
}
.check{
    width: 30px;
}
.check input{
    width: 30px;
}
.img{
    width: 100px;
    height: 90px;
    margin-left: 20px;
}
.right{
    width: 100%;
    margin-left: 30px;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}
.right div{
    display: flex;
    justify-content: space-between;
}
.right .new{
    color: red;
}
.right .old{
    color: gray;
}
.right .jian{
    width: 20px;
    height: 20px;
    background-color: #f4f4f4;
    border-radius: 50%;
    text-align: center;
}
.right .jia{
    width: 20px;
    height: 20px;
    background-color: #f4f4f4;
    border-radius: 50%;
    text-align: center;
}
.li-bottom{
    display: flex;
    align-items: center;
    justify-content: space-between;
}
.li-bottom-left{
    margin-left: 40px;
}
.li-bottom-left .addprice{
    border: 1px solid red;
    color: red;
}
.li-bottom-left .other{
    color: gray;
}
.allprice{
    width: 100%;
    height: 60px;
    background-color: white;
    position: fixed;
    bottom: 0;
    display: flex;
    justify-content: space-around;
    align-items: center;
    font-size: 14px;
}
.total{
    font-size: 15px;
    font-weight: 600;
}
.settlement{
    width: 140px;
    height: 40px;
    background-color:red;
    border-radius: 20px;
    color: white;
    line-height: 40px;
    text-align: center;
}
</style>