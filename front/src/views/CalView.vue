<template>
    <div class="cal">
        <div class="cal_banner">
            <img src="../assets/images/cal/cal_banner.png" alt="" >
            <div class="cal_banner_textbackground">
                <h1>食物熱量計算</h1>
            </div>
        </div>
        <breadcrumb :breadcrumb="yourBreadcrumbData"></breadcrumb>

        <div ref="calCountainer" class="cal_countainer">
            <h2>食物熱量計算</h2>
            <p>食物熱量，事實上是製做出該食物所需要的材料的熱量總和。簡單來說，炒飯的用料有飯、蛋、蔥花、油，熱量就來自於這些用料的總和。食物的熱量主要受到材料、材料的量及烹調等方法的影響。食物的用料越多越複雜，熱量就越高。</p>
            <div class="cal_inputs">
                <div class="cal_food">
                    <p>食物(種類)</p>
                    <select class="cal_food_select" v-model="chooseFood" ref="chooseFood">
                        <option>請選擇食物種類</option>
                        <option value="10">全穀雜糧類</option>
                        <option value="10">豆魚蛋肉類</option>
                        <option value="10">乳品類</option>
                        <option value="10">蔬菜類</option>
                        <option value="10">水果類</option>
                        <option value="10">油脂與堅果類</option>
                    </select>
                </div>
                <div class="cal_g">
                    <p>份量(公克)</p>
                    <input class="cal_g_input" v-model="portionSize" type="number">
                </div>
            </div>
            <ul class="cal_measurement_step">
                <li>1.拆解成食物的原料。</li>
                <li>2.分別估算各項原料的重量。</li>
                <li>3.算出份量，查照食物代換表，找到食物的重量，公式=重量除以食物的重量。</li>
                <li>4.查照食物代換表，找到熱量，公式=每份熱量X份數。</li>
                <li>5.各項原料相加計算。</li>
            </ul>
            <button class="btn-product"
            @click="calCalculate">立即計算</button>
        </div>


        <div ref="calHealthyRecommend" class="cal_healthy_recommend">
            <h2>您查詢的<span>食物熱量為 {{cal}} 大卡</span></h2>
            <h3>為你推薦專屬商品</h3>
            <p>以下食品的熱量不僅符合您的目前cal的需求，GI值也非常健康！有效穩定血糖、幫助減脂！！！</p>
            <div class="recommend_wall">
                <div class="cal_recommend_card"
                v-for="(item, index) in displatdata"
                :key="item"
                >
                    <span class="cal_tag">#NEW</span>
                    <div class="cal_card_img">
                        <img :src="getImageUrl(item.image)" alt="item.name">
                    </div>
                    <p class="cal_card_title">{{ item.name }}</p>
                    <p class="cal_card_price">{{ item.price }}</p>
                    <router-link :to="{name: 'productinfo',params: {id: item.id}}" class="btn-product">查看商品詳情</router-link>
                </div>
            </div>
            <button class="btn-product"
            @click="calRecalculate">重新計算</button>
        </div>
    </div>
</template>
<script>
import { RouterLink, RouterView } from 'vue-router'
import Breadcrumb from '@/components/Breadcrumb.vue';
import VegetableCard from "@/components/VegetableCard.vue";
export default {
    data() {
        return {
            chooseFood:'',
            portionSize:null,
            cal:null,
            displatdata:[
                {
                    index:1,
                    id: 2001,
                    name: "有機雞蛋",
                    price: "$100",
                    image: "product/eggs-cover.png",
                    type: "egg"
                },
                {
                    index:2,
                    id: 3001,
                    name: "食用油",
                    price: "$300",
                    image: "product/oil-cover.jpg",
                    type: "oil"
                },
                {
                    index:3,
                    id: 4001,
                    name: "水產養殖鮮魚",
                    price: "$500",
                    image: "product/fish-cover.png",
                    type: "fish"
                },
            ],
            yourBreadcrumbData: [
                { text: '首頁', to: '/' },
                { text: '健康小幫手' , to: ''},
                { text: '食物熱量計算', active: true }
            ],
        };
    },
    methods: {
        getImageUrl(paths) {
            return new URL(`../assets/images/${paths}`, import.meta.url).href;
        },
        calCalculate(){
            if( this.portionSize && this.chooseFood ){
                this.cal = this.portionSize * parseFloat(this.chooseFood);
                this.$refs.calHealthyRecommend.style.display = "flex";
                this.$refs.calCountainer.style.display = "none";
            }else{
                alert("請輸入正確數值");
            }
        },
        calRecalculate(){
            this.$refs.calCountainer.style.display = "flex";
            this.$refs.calHealthyRecommend.style.display = "none";
            this.portionSize = null;
            this.chooseFood = '';
            this.cal = null;
        }
    },
    mounted() { // Vue 實例創建之後立即被調用
        
    },
    beforeDestroy() {
    },
    components: {
        RouterLink,
        RouterView,
        Breadcrumb,
        VegetableCard,
    },
}
</script>

<style lang="scss">
    @import "@/assets/scss/page/_cal.scss";
</style>


