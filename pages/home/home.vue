<template>
	<view>
		<!-- 轮播图 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item>
				<view class="swiper-item">1111</view>
			</swiper-item>
			<swiper-item>
				<view class="swiper-item">222</view>
			</swiper-item>
		</swiper>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			};
		}
	}
</script>

<style lang="less">
	swiper {
		height: 330rpx;
		background-color: #007AFF;
	}
</style>
