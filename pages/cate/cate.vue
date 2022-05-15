<template>
	<view>
		<my-search @goSearch="goSearch"></my-search>
	<view class="scroll-container" ref="scr" :style="{height:scrollHeight+'px'}">
		<scroll-view scroll-y="true" style="width: 250rpx;">
			<view v-for="item,index in cateList" :key="index" class="left-scroll-item"
				:class="index==active?'active':''" @click="activeChang(index)">
				{{item.cat_name}}
			</view>
		</scroll-view>
		<scroll-view :scroll-top="scrollTop" scroll-y="true" style="height: 100%;width: 500rpx;">
			<view class="childern_two" v-for="item,index in cateList[active].children" :key="index">
				<view class="children_name">
					/{{item.cat_name}}/
				</view>
				<view @click="goGoodsList(item1)" class="childern_item" v-for="item1,index1 in item.children" :key="index1">
					<image :src="item1.cat_icon" mode="widthFix"></image>
					<view>{{item1.cat_name}}</view>
				</view>
			</view>

		</scroll-view>
	</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				scrollHeight: 0,
				cateList: {},
				active: 0,
				scrollTop: 0 
			};
		},
		onLoad() {
			this.scrollHeight = uni.getSystemInfoSync().windowHeight-50
			this.getCateList()
			// console.log(document.querySelector('.scroll-container').clientHeight);
			// console.log(this.$refs)
		},
		methods: {
			async getCateList() {
				const result = await uni.$http.get('/api/public/v1/categories')
				if (result.data.meta.status == 200) {
					this.cateList = result.data.message
				} else {
					uni.$showMsg()
				}
			},
			activeChang(index) {
				this.active = index
				this.scrollTop = this.scrollTop===0?1:0 //重置滚动条的位置，如果为0重新赋值为0 scroll-top 不会变化
			},
			goGoodsList(item){
				uni.navigateTo({
					url:'/subpkg/goods_detail/goods_detail?cid='+item.cat_id
				})
			},
			goSearch(){
				uni.navigateTo({
					url:'/subpkg/search/search'
				})
			}
		}
	}
</script>

<style lang="scss">
	view{
		box-sizing: border-box;
	}
	.scroll-container {
		display: flex;
	}

	.left-scroll-item {
		background-color: #F7F7F7;
		line-height: 60px;
		text-align: center;
		font-size: 12px;
	}

	.active {
		background-color: #FFFFFF;
		position: relative;

		&::before {
			content: ' ';
			display: block;
			width: 3px;
			height: 30px;
			background-color: #C00000;
			position: absolute;
			top: 50%;
			left: 0;
			transform: translateY(-50%);
		}
	}
	.childern_two{
		display: flex;
		flex-wrap: wrap;
		justify-content: space-between;
		.children_name{
			width: 500rpx;
			text-align: center;
			font-size: 14px;
			margin-bottom: 5px;
		}
		.childern_item{
			image{
				width: 100rpx;
			}
			width: 33.3%;
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			font-size: 12px;
			margin-bottom: 10px;
		}
	}
</style>
