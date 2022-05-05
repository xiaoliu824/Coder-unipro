<template>
	<view>
		<!-- 轮播图 -->
		<swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
			<swiper-item v-for="(item, i) in swiperList" :key="i">
				<!-- <view class="swiper-item"> -->
				<navigator class="swiper-item" :url="'/subPackge/goods_detail/goods_detail?goods_id='+item.goods_id">
					<image :src="item.image_src"></image>
				</navigator>
				<!-- </view>/ -->
			</swiper-item>
		</swiper>
		<!-- 分类导航区域 -->
		<view class="nav-list">
			<view class="nav-list-item" v-for="(item,index) in navList" :key="index" @click="navToView(item)">
				<image class="nav-img" :src="item.image_src"></image>
			</view>
		</view>
		<!-- 楼层区域 -->
		<view class="floor-list">
			<!-- 楼层每一项 -->
			<view class="floor-item" v-for="(item,index) in floorList" :key="index">
				<!-- 楼层的标题 -->
				<image class="floor-title" :src="item.floor_title.image_src"></image>
				<!-- 楼层图片区域 -->
				<view class="floor-img-box">
					<!-- 左边大盒子 -->
					<navigator class="left-img-box" :url="item.product_list[0].url">
						<image :src="item.product_list[0].image_src"
							:style="{width:item.product_list[0].image_width + 'rpx' }" mode="widthFix"></image>
					</navigator>
					<!-- 右侧小盒子 -->
					<view class="right-img-box">
						<navigator class="right-img-item" v-for="(itemR,index) in item.product_list" :key="index"
						    :url="itemR.url"
							v-if="index !== 0">
							<image :src="itemR.image_src" :style="{width: itemR.image_width + 'rpx'}" mode="widthFix">
							</image>
						</navigator>
					</view>
				</view>
			</view>

		</view>
	</view>
</template>

<script>
	import {
		Homeurl
	} from '../../api/requesturl.js'
	export default {
		data() {
			return {
				swiperList: [], // 轮播图
				navList: [], // 分类列表
				floorList: [], // 楼层列表
			};
		},
		onLoad: function() {
			this.getSwiperList()
			this.getNavList()
			this.getFloorList()
		},
		methods: {
			// 获取轮播图数据
			async getSwiperList() {
				let res = await this.handlerRequest(Homeurl.swiper)
				this.swiperList = res
			},
			// 获取分类列表
			async getNavList() {
				let res = await this.handlerRequest(Homeurl.catelist)
				this.navList = res
			},
			// 获取楼层列表
			async getFloorList() {
				let res = await this.handlerRequest(Homeurl.floorList)
				res.forEach(item => {
					item.product_list.forEach(prod => {
						prod.url = '/subPackge/goods_list/goods_list?' + prod.navigator_url.split("?")[1]
					})
				})
				console.log(res)
				this.floorList = res
			},
			// 跳转
			navToView(itemObj) {
				if (itemObj.name == "分类") {
					uni.switchTab({
						url: '/pages/cate/cate'
					})
				}
			},
			// 请求函数
			async handlerRequest(url) {
				let {
					data: res
				} = await uni.$http.get(url);
				if (res.meta.status !== 200) return uni.$showMsg()
				return res.message
			}
		}
	}
</script>

<style lang="less">
	swiper {
		height: 330rpx;

		.swiper-item,
		image {
			width: 100%;
			height: 100%;
		}
	}

	.nav-list {
		display: flex;
		justify-content: space-around;
		margin: 15rpx 0;

		.nav-img {
			width: 128rpx;
			height: 140rpx;
		}
	}

	.floor-title {
		height: 60rpx;
		width: 100%;
		display: flex;
	}

	.floor-img-box {
		display: flex;

		.right-img-box {
			display: flex;
			flex-wrap: wrap;
			justify-content: space-around;
		}
	}
</style>
