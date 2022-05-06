<template>
	<view>
		<view class="scroll-view-container">
			<!-- 左侧滑动区域 -->
			<scroll-view class="left-scroll-view" scroll-y :style="{height: phoneH + 'px'}">
				<block v-for="(item,index) in cateList" :key="index">
					<view :class="['left-scroll-view-item', index === active ? 'active' : '']"
					 @click="activeChange(index)"
					>{{item.cat_name}}</view>
				</block>
				
			</scroll-view>
			<!-- 右侧滑动区域 -->
			<scroll-view scroll-y :style="{height: phoneH + 'px'}" :scroll-top="scrollTop">
				<view class="cate-lv" v-for="(cateT,index) in cateTwoList" :key="index">
					/ {{cateT.cat_name}} /
					<!-- 三级分类 -->
					<view class="cate-lv-list">
						<!-- 三级分类项 -->
						<view class="cate-lv-list-item" v-for="(cateC, indexC) in cateT.children" :key="indexC"
						 @click="goItemDetails(cateC)"
						>
							<!-- 图片 -->
							<image :src="cateC.cat_icon" ></image>
							<!-- 文本 -->
							<text>{{cateC.cat_name}}</text>
						</view>
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
				phoneH: 0,
				cateList: [], // 分类数据
				active: 0,
				cateTwoList: [], // 二级分类
				scrollTop: 0
			};
		},
		onLoad: function() {
			// 获取当前设备的信息
			let phoneHeight = uni.getSystemInfoSync();

			this.phoneH = phoneHeight.windowHeight;
			this.getCateList()
			
		},
		methods:{
			async getCateList() {
				let {data: res} = await uni.$http.get('/api/public/v1/categories');
				// console.log("Coder",res)
				if(res.meta.status !== 200) return uni.$showMsg()
				this.cateList = res.message
				this.cateTwoList = this.cateList[0].children
			},
			activeChange(Index) {
				this.active = Index
				this.cateTwoList = this.cateList[Index].children
				console.log(this.cateTwoList)
				// 切换分类时,将页面滚动到顶部
				// 防止两次赋值一致，不生效所以给1px(用户体验不到)
				this.scrollTop = this.scrollTop === 0 ? 1 : 0
			},
			goItemDetails(item) {
				console.log("点击了某一项",item)
				uni.navigateTo({
					url:"/subPackge/goods_list/goods_list?cid="+item.cat_id
				})
			}
		}
	}
</script>

<style lang="less">
.scroll-view-container {
	display: flex;
	.left-scroll-view {
		width: 120px;
		.left-scroll-view-item {
			background-color: #F7F7F7;
			line-height: 60px;
			text-align: center;
			font-size: 12px;
			
			&.active {
				background-color: #FFFFFF;
				position: relative;
				&::before {
					content: " ";
					display: block;
					width: 3px;
					height: 30px;
					background-color: red;
					position: absolute;
					top: 50%;
					left: 0;
					transform: translateY(-50%);
				}
			}
		}
	}
	.cate-lv {
		font-size: 12px;
		padding: 15px 0;
		font-weight: bold;
		text-align: center;
		.cate-lv-list {
			display: flex;
			flex-wrap: wrap;
			margin-top: 10px;
			.cate-lv-list-item {
				width: 33.3%;
				margin-bottom: 10px;
				display: flex;
				flex-direction: column;
				align-items: center;
				image {
					width: 60px;
					height: 60px;
				}
				text {
					font-size: 12px;
				}
			}
		}
	}
}
</style>
