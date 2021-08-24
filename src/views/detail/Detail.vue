<template>
    <div id="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
        <!-- <scroll class="comtent"> -->
            <detail-swiper :top-images="topImages"></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
        <!-- </scroll> -->
    </div>
</template>

<script>
import DetailNavBar from './childComps/DetailNavBar.vue'
import DetailSwiper from './childComps/DetailSwiper.vue'
import DetailBaseInfo from './childComps/DetailBaseInfo.vue'
import DetailShopInfo from './childComps/DetailShopInfo.vue'

// import Scroll from 'components/common/scroll/Scroll'

import {getDetail,Goods,Shop} from 'network/detail'

export default {
    name: 'Detail',
    components:{
        DetailNavBar,
        DetailSwiper,
        DetailBaseInfo,
        DetailShopInfo,
        // Scroll,
    },
    data(){
        return {
            iid:0,
            // res:null,
            topImages:[],
            //将详情信息整合在一个对象中 
            goods:{},
            shop:{}
        }
    },
    created(){
        //保存获取到的iid
        this.iid = this.$route.params.iid

        //请求数据
        getDetail(this.iid).then(res => {
            console.log(res);
            // this.res = res
            // 1.获取顶部轮播的图片数据
            const data = res.result;
            this.topImages = data.itemInfo.topImages;

            this.goods = new Goods(data.itemInfo,data.columns,data.shopInfo.services)

            //店铺信息
            this.shop = new Shop(data.shopInfo)

        })

    }
}
</script>

<style scoped>
    #detail{
        position: relative;
        z-index: 9;
        background-color: #fff;
        height: 100vh;
    }
    .detail-nav{
        position: sticky;
        top: 0;
         /* position: fixed; */
        z-index: 9;
        background-color: #fff;
    }
    .comtent{
        height: calc(100%-44px);
    }
</style>