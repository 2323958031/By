<template>
	<view class="box">
		<view class="ssk">
			<view class="ssl">
				<image class="fdj" src="../../../static/tool/搜索.png" mode=""></image>
				请输入你想要的商品
			</view>
		</view>
		<!-- 头部导航栏 -->
		<scroll-view class="topnav" scroll-x="true" scroll-left="120" show-scrollbar >
			<view :class="ins==-1?'act remen':'remen'" @click="erji(-1)">
				热门
			</view>

			<view :class="ins==i?'act remen':'remen'" v-for="(item,i) in erjinav" key="i" @click="erji(i)">
				{{item.categoryName}}
			</view>
		</scroll-view>
		<!-- banner图 -->
		<swiper class="swiper" circular indicator-dots="true" autoplay="true" interval="5000" indicator-active-color="#f7b200">
			<swiper-item v-for="(swiper,index) in imgs" key="index">
				<image :src="swiper.newImageUrl"></image>
			</swiper-item>
		</swiper>

		<!-- 轮播图下面 -->
		<view class="bannerxia">
			<view class="fen">
				大牌品质/
			</view>
			<view class="fen">
				工厂价格/
			</view>
			<view class="fen">
				分期支付/
			</view>
			<view class="fen">
				顺丰包邮/
			</view>
			<view class="fen">
				无忧退款
			</view>
		</view>
		<view class="bannxia2">
			<view v-for="(item,index) in bannxia" :key="index">
				<view class="xiaico">
					<image class="icos" :src="item.icon" mode=""></image>
					<view class="titl">
						{{item.title}}
					</view>
				</view>
			</view>
		</view>


		<!-- 产品列表 -->
		<view class="lists"  >
			<view class="shops" v-for="(item,index) in arr" :key="index" >
				<view class="shopa" v-for="(it,i) in item.data" :key="i"  @click="navTo(it.routerParams.suId)">
					<image class="shopimg" :src="it.image" mode=""></image>
					<view class="price">
						￥{{it.priceStr}}
					</view>
						<view class="labels" >
							<view class="content" v-for="(eme,ia) in it.labels" :key="ia">
								{{eme.content}}
							</view>
						</view>
					<view class="mainTitle">
						{{it.mainTitle}}
					</view>
					<view class="thirdContent">
						{{it.thirdContent}}
					</view>
				</view>
			</view>
			<div style="text-align: center; line-height: 50rpx;" v-if="loading">加载中...</div>
		</view>
		</view>
</template>

<script setup>
	import {
		onMounted,
		ref
	} from 'vue'
	import {
		onLoad,onReachBottom
	} from '@dcloudio/uni-app'
import { logInfo } from '../../../common/sju.base';

	let arr = ref([])
	let erjinav = ref([])
	let imgs = ref([])
	let bannxia = ref([])
	let loading=ref(false)
	let page=ref(2)
	let ins=ref(-1)
	onLoad((op) => {
		uni.request({
			url: "http://192.168.212.51:3232/home",
			success(res) {
				erjinav.value = res.data.homeData.oneLevelCategoryList
				imgs.value = res.data.homeData.banners
				bannxia.value = res.data.homeData.operationNavigation
				console.log(res.data.floorData.blockList[1].block, "123");
				arr.value = res.data.floorData.blockList[1].block
			}
		})
	})
	let  erji=(e)=>{
		ins.value=e
	}
	onReachBottom((e)=>{
		loading.value=true
		page.value++
		uni.request({
		// 	// method:'POST',
			url: `http://192.168.212.51:3232/floorlist?pageIndex=${page.value}`,
			data:{
				
			},
			success(res) {

				arr.value=arr.value.concat(res.data.data.blockList[0].block)
				console.log(res.data.data.blockList[0].block,"下拉");
				console.log(arr.value);
				if(res.data.data){
						loading.value=false
				}
			}
		})

	})
	let navTo=(id)=>{
		// uin.navigateTo({
			
		// })
		uni.navigateTo({
			url:`/sub_goods/pages/goodsDetail/goodsDetail?id=${id}`
		})
	}

</script>

<style lang="scss" scoped>
	.box{padding-top: 50rpx;
		width: 100vw;
		// height: 90vh;
	}
	// 搜索
	.ssk {
		width: 98vw;
		margin: 20rpx auto;
		overflow: hidden;
		text-align: left;
		line-height: 60rpx;

		.ssl {
			border: 1px solid #ccc;
			border-radius: 32rpx;

		}

		.fdj {
			width: 40rpx;
			height: 40rpx;
			margin: 10rpx;
			float: left;
		}
	}

	// 二级菜单栏
	.topnav {
		width: 96vw;
		height: 100rpx;
		margin: auto;
		line-height: 30rpx;
		white-space: nowrap;
		text-align: center;
	.act{
		border-bottom: 1px solid red;
	}
		.remen {
			// width: 200rpx;
			// float: left;
			display: inline-block;
			margin-left: 10rpx;
			padding: 15rpx;
		}
	}


	/* 轮播图 */
	.swiper {
		$swiper-width: 100%;
		$swiper-height: 400upx;
		$text-align: center;
		width: $swiper-width;
		height: $swiper-height;

		image {
			width: $swiper-width;
			height: $swiper-height;
		}
	}

	// 轮播图下面文
	.bannerxia {
		width: 96vw;
		margin: auto;
		display: flex;

		.fen {
			padding: 10rpx;
			margin: 5rpx;
		}
	}

	.bannxia2 {
		width: 96vw;
		margin:10rpx auto;
		background: #fff;
		height: 150rpx;
		.xiaico {
			width: 20%;
			float: left;
			text-align: center;

			.icos {
				margin: 10rpx 20rpx;
				width: 50rpx;
				height: 50rpx;
			}
		}
	}

	// 商品列表
	.lists {
		width: 96vw;
		position: relative;
		margin:50rpx auto;
		// bottom: 10vh;
		// height: 90vh;
		overflow: hidden;
		.shops {
			padding: 20rpx 0;
			background: #fff;
			width: 47.5vw;
			margin-left: 0.5vw;
			// width: 350rpx;
			float: left;
		margin-bottom: 2rpx;
			// margin-right: 5%;

			.shopa {
				width: 100%;
				
				.shopimg {
					width: 100%;
					height: 400rpx;
				}
				.price{
					color: #f7a701;
					font-size: 34rpx;
				}
				.labels{
					margin: 20rpx;
					overflow: hidden;
					.content{
						margin-right: 20rpx;
						width: 100rpx;
						float: left;
						text-align: center;
						background: rgb(255, 255, 255);
						color: rgb(251, 76, 129);
						border: 0.015rem solid rgb(251, 76, 129);
						border-radius: 2px;
					}
				}
				.mainTitle{
					width: 100%;
					overflow: hidden;
					color: #4a4a4a;
					line-height: 30rpx;
					text-overflow: ellipsis;
					white-space: nowrap;
				}
				.thirdContent{
					margin-top: 15rpx;
					color: #bbbbbb;
					font-size: 0.2rem;
				}
			}

		}
	}


	/* 导航栏 */
	.menu {
		width: 100%;
		padding-top: 20upx;
		margin-top: 20upx;
		height: 300upx;
		background-color: var(--accent-bg-color);
		display: flex;
		flex-wrap: wrap;

		// justify-content: space-between;
		.menuItem {
			display: flex;
			flex-direction: column;
			justify-content: center;
			align-items: center;
			width: 25%;
			height: 130upx;
			margin-bottom: 20upx;

			image {
				width: 80upx;
				height: 80upx;
			}

			.iconfont {
				font-size: 80upx;
				color: var(--primary-color);
				// color: transparent;
				// background: radial-gradient(circle, var(--assist-color) 0%, var(--primary-color) 100%);
				// background-clip: text;
			}

			text {
				font-size: 26upx;
				color: $text-color;
				margin-top: 10upx;
			}
		}
	}

	// 产品推荐
	.title-box {
		width: calc(100% - 40upx);
		height: 100upx;
		line-height: 100upx;
		margin: 20upx 0;
		padding: 0 20upx;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		background-color: var(--accent-bg-color);
		border-bottom: 1upx solid var(--border-color);

		.title:before {
			content: '';
			width: 6upx;
			height: 40upx;
			margin-right: 10upx;
			display: inline-block;
			vertical-align: middle;
			border-radius: 6upx;
			background-color: var(--primary-color);
		}

		.iconfont {
			font-size: 60upx;
			color: var(--primary-color);
		}
	}

	// 商品
	.goods-item {
		width: 100%;
		margin-bottom: 20upx;
		background-color: #FFFFFF;
		border-radius: 10upx;
		overflow: hidden;

		// 商品图片
		.goods-image {
			width: 100%;
		}

		// 标题
		.goods-title {
			padding: 20upx;
			height: 100%;

			// 标签
			.tag {
				padding: 4upx 16upx;
				margin-right: 10upx;
				border-radius: 6upx;
				font-size: 24upx;
				color: #FFFFFF;
				background: linear-gradient(to right, #fe3b0f, #fc603a);
			}

			// 标题
			.title {
				float: left;
				font-size: 26upx;
				line-height: 40upx;
				margin-right: 10upx;
			}
		}

		// 价格信息
		.price-box {
			width: calc(100% - 40upx);
			line-height: 60upx;
			padding: 0 20upx;
			display: flex;
			flex-wrap: wrap;
			align-items: center;
			justify-content: space-between;

			// 原价
			.price {
				display: flex;
				align-items: center;
				color: $price-color;
				font-size: 22upx;
				text-decoration: line-through;

				span {
					font-size: 28upx;
				}
			}

			// 当前价格
			.current-price {
				color: $price-color;

				span {
					font-size: 34upx;
					font-weight: 600;
				}
			}
		}
	}

	.loadingText {
		width: 100%;
		line-height: 80upx;
		text-align: center;
		color: #c0c0c0;
	}
</style>