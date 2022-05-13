<template>
	<view>
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="item,index in swiperList" :key="index">
				<view class="swiper-item" @click="goGoodsDetail(item.goods_id)">
					<image :src="item.image_src"></image>
				</view>
			</swiper-item>
		</swiper>
		
		<view class="nav-list">
			<view class="nav-item" v-for="item,index in navList" :key="index" @click="switchTab(item)">
				<image class="item-image" :src="item.image_src"></image>
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				swiperList:{},
				navList:{}
			};
		},
		onLoad() {
			this.getSwiperList()
			this.getNavList()
		},
		methods:{
			async getSwiperList(){
				const result = await uni.$http.get('/api/public/v1/home/swiperdata')
				if(result.data.meta.status ==200){
					this.swiperList = result.data.message
				}else{
					uni.$showMsg()
				}
			},
			goGoodsDetail(id){
				uni.navigateTo({
						url:`/subpkg/goods_detail/goods_detail?goods_id=${id}`
				})
			},
			async getNavList(){
				const result = await uni.$http.get('/api/public/v1/home/catitems')
				if (result.data.meta.status==200) {
					this.navList = result.data.message
				} else{
					uni.$showMsg()
				}
			},
			switchTab(item){
				console.log(item);
				if(item.name=="分类"){
					uni.switchTab({
						url:'/pages/cate/cate'
					})
				}
			}
		}
	}
</script>

<style lang="scss">
swiper{
	height: 330rpx;
	.swiper-item,image{
		width: 100%;
		height: 100%;
	}
}
.nav-list{
	display: flex;
	justify-content: space-around;
	margin: 15px 0;
	.item-image{
		height: 140rpx;
		width: 128rpx;
	}
}
</style>
