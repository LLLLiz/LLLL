<template>
    <briup-fulllayout title="订单确认">

    服务地址：
    <van-dropdown-menu>
        <van-dropdown-item v-model="form.addressId" :options="addresses" />
    </van-dropdown-menu>
    订单详情：
        <div style="padding: 0 2em">
            <p>服务名称：{{$router.query.name}}</p>
            <p>服务价格：{{$router.query.price}}</p>
            <p>服务数量：1</p>
            <p>服务小计：{{$router.query.price*1}}</p>
        </div>
        <div style="position:fixed;bottom:0;width:100%;">
            <van-button block type="primary" @click="submitHandler">提交订单</van-button>
        </div>
        
    </briup-fulllayout>
</template>
<script>
import{mapState}from'vuex'
import{get,post_obj_array}from'../../../http/axios'
export default{
    data(){
        return{
            addressId:0,  //服务地址id
            addresses:[],
            orderLines:[]

        }   
    },
    //jiazaidizhixinxi
    created(){
        this.loadAddress();
        //初始化订单向 将购买的产品放入购物车
        let orderLine={
            number:1,
            price:$this.$router.price,
            productId:this.$router.id
        }
        
        this.orderLines.push(orderLine);
    },
    //jusuanshuxing  yingshe info  xinxi
    computed:{
        ...mapState("user",["info"]),
        options:function(){
            //将addresses的数据计算为一个新的数组返回
            return this.addresses.map(item=>{
                return{
                    text:item.province+" "+item.city+" "+item.area+" "+item.address,
                    value:item.id
                }
            })
        }
    },
    methods:{
        //提交订单
        submitHandler(){
            
            let url = "/order/save"
            let data ={
                customerId:this.info.id,
                addressId:this.addressesId,
                orderLines:this.orderLines
            }
            console.log(data);
            post_obj_array()
        },
        //chaxundizhixinxi
        loadAddress(){
            let id=this.info.id;
            let url = "/address/findByCustomerId?id="+id
            get(url).then((response)=>{
                this.addresses=response.data;
                //将查询到的第一个id赋值给data中的id作为默认id
                this.addressId=this.addresses[0].id;
            })
        },
    }
}
</script>