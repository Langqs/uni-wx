<template>
	<view class="scroll-container" ref="scr" :style="{height:scrollHeight+'px'}">
		<scroll-view scroll-y="true" style="width: 250rpx;">
			<view v-for="item,index in 100" :key="index" class="left-scroll-item active">iiiiiii</view>
		</scroll-view>
		<scroll-view scroll-y="true" style="height: 100%;">
			<view v-for="item,index in 100" :key="index">xxxxxxx</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				scrollHeight:0,
			};
		},
		onLoad() {
			this.scrollHeight = uni.getSystemInfoSync().windowHeight
			// console.log(document.querySelector('.scroll-container').clientHeight);
			// console.log(this.$refs)
		}
	}
</script>

<style lang="scss">
.scroll-container{
	display: flex;
}
.left-scroll-item{
	background-color: #F7F7F7;
	line-height: 60px;
	text-align: center;
	font-size: 12px;
}
.active{
	background-color: #FFFFFF;
	position: relative;
	&::before{
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
</style>
