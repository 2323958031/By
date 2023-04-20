<template>
	<view :style="Theme">
		<!-- <uni-nav-bar :statusBar="true" leftIcon="arrowleft" fixed="true"  title="商品详情" /> -->
		<!-- 商品图片轮播 -->
		<swiper class="swiper" autoplay="true" interval="4000"
			duration="1000" :current="current" circular="true" @change="swiperChange">
			<swiper-item v-for="(item,index) in productInfo.imgList" :key="index">
				<image class="images" :src="item.largeImg"  mode="aspectFit"></image>
				<view class="laber">{{index+1}} / {{productInfo.imgList.length}}</view>
			</swiper-item>
		</swiper>
		
		<!-- 商品名称 -->
		<view class="goods-info">
			<view class="goods-price">
				<view class="present-price">
					￥<text v-if="selectedSku">{{selectedSku.togGroupPriceStr}}</text>
					<view class="shou-chang">
						<image src="../../../static/img/爱心.png" mode=""></image>
						收藏
					</view>
				</view>
				<view class="original-price" v-if="selectedSku">{{selectedSku.supplierBackground}}</view>
			</view>
			<view class="goods-name">{{productInfo.productName}}</view>
			
			<view class="goods-amount">
				<view class="sold">{{productInfo.productSale}}</view>
				<!-- <view class="sold">库存剩余{{productInfo.stockQty}}件</view> -->
			</view>

		</view>
		<view class="baozheng">
			<view class="bao-zzz">
				<image src="../../../static/img/icon_对勾中.png" mode=""></image>
				<text class="bao-ttt">
					七天无理由退货
				</text>
			</view>
			<view class="bao-zzz">
				<image src="../../../static/img/icon_对勾中.png" mode=""></image>
				<text class="bao-ttt">
					先行赔付
				</text>
			</view>
			<view class="bao-zzz">
				<image src="../../../static/img/icon_对勾中.png" mode=""></image>
				<text class="bao-ttt">
					超时赔付
				</text>
			</view>
			<view class="bao-zzz">
				<image src="../../../static/img/icon_对勾中.png" mode=""></image>
				<text class="bao-ttt">
					顺丰包邮
				</text>
			</view>
			<view class="bao-zzz">
				<text class="bao-ttt" style="margin-left: 30rpx;" @click="tuihuo">
				···
				</text>
			</view>
		</view>
		<view class="dibu" ref="fufu"  v-if="flas">
			<img src="../../../static/img/QQ截图20230420110451.png" alt="">
			<!-- <image class="fuwu" src="../../../static/img/QQ截图20230420110451.png" mode=""></image> -->
			<button class="fuwuqd" @click="ddd">确定</button>
		</view>
		<!-- 属性规格 -->
		<!-- <view class="tool-box">
			<view class="tool-item" @click="$refs['sjuSpecs'].show()">
				<view class="item-left">
					<view class="item-title">规格</view>
					<view class="item-center">
						{{ skuStr }}
					</view>
				</view>
				<i class="iconfont icon-right"></i>
			</view>
			<view class="tool-item" @click="$refs['sjuCoupon'].show()">
				<view class="item-left">
					<view class="item-title">领券</view>
					<view class="item-coupon">
						<view class="coupon-box" v-for="(item,index) in couponCenterList.slice(0,2)" :key="index">
							{{item.describe}}
						</view>
					</view>
				</view>
				<i class="iconfont icon-right"></i>
			</view>
			<view class="tool-item" @click="$refs['sjuService'].show()">
				<view class="item-left">
					<view class="item-title">服务</view>
					<view class="item-center">
						{{serviceList.map(item => { return item.title }).join(",")}}
					</view>
				</view>
				<i class="iconfont icon-right"></i>
			</view>
			<view class="tool-item" @click="$refs['sjuParams'].show()">
				<view class="item-left">
					<view class="item-title">
						参数
					</view>
					<view class="item-center">
						{{ paramText }}
					</view>
				</view>
				<i class="iconfont icon-right"></i>
			</view>
		</view> -->
		
		<!-- 评论模块 -->
		<view class="comment">
			<!-- 评论标题 -->
			<view class="title-box">
				<view class="title">
					<!-- 宝贝评价<text>({{pl.count}})</text> -->
				</view>
				<view class="icon-box" @click="navTo(`/goods/evaluateList?code=${this.productCode}`)">
					<view class="icon-name">查看全部</view>
					<i class="iconfont icon-right"></i>
				</view>
			</view>
			<!-- 评价列表 -->
			<!-- <view class="comment-list" v-for="(item,index) in evaluate.list.slice(0,2)" :key="index"> -->
				<!-- 评论用户 -->
<!-- 				<view class="item-user">
					<image class="user-image" :src="item.image" ></image>
					<view class="user-info">
						<view class="user-name">{{filters_name(item.name)}}</view>
						<view class="date">{{item.date}}</view>
					</view>
				</view>
				<view class="comment-content">
					{{item.content}}
				</view>
				<view class="comment-images">
					<image v-for="(items,index) in item.imagesList" :key="index" 
						:src="items" @click="previewImage(item.imagesList,index)" mode="aspectFit"></image>
				</view>
			</view> -->
		</view>
		
		<!-- 商品详情 -->
		<view class="products-introduction">
		  <view class="title">
		    <text>商品详情</text>
		  </view>
		  
		  
		  
		  
		  
		  <view class="cpmc">
			<view class="cpmc-cpmc" v-for="(item,i) in  productInfo.productTopDetail" :key="index">
			<text class="left">{{item.name}}</text>	
			<text class="right">{{item.desc}}</text>
			</view>
		  </view>
		  
		  <view class="product-content" v-html="productInfo.productButtomDetail">
			  
		  </view>
		</view>
		
		<!-- 商品导航 -->
		<view class="goods-nav">
			<view class="nav-left">
				<view class="item-box">
					<i class="iconfont icon-kefu"></i>
					<view>客服</view>
				</view>
				<view class="item-box" @click="navTo(`/store/storeDetail?code=${productInfo.storeCode}`)">
					<i class="iconfont icon-shop"></i>
					<view>商铺</view>
				</view>
				<view class="item-box" @click="sjuNav.switchTab('/tab/cart')">
					<i class="iconfont icon-cart"></i>
					<view>购物车</view>
				</view>
			</view>
			<view class="nav-right">
				<view class="add-btn"
				 >
				加入购物车
				</view>
				<view  class="buy-btn"
				 >
				立即购买
				</view>

			</view>
		</view>
		<!-- 服务弹窗 -->
		<!-- <sju-service ref="sjuService" :serviceList="serviceList"></sju-service> -->
		<!-- 优惠券弹窗 -->
	<!-- 	<sju-coupon ref="sjuCoupon" 
		:couponList="couponCenterList"
		@receive='couponReceive'></sju-coupon> -->
		<!-- 属性规格 -->
	<!-- 	<sju-specs ref="sjuSpecs" 
		:goodsInfo="productInfo" 
		:skuList="skuList" 
		:priceList="priceList" 
		:goodType="goodsType"
		bgColor="var(--primary-color)"
		@onAdd ="onAdd"
		@onBuy ="onBuy"
		@onChange="onSkuChange">
		</sju-specs> -->
		<!-- 商品参数 -->
	<!-- 	<sju-params ref="sjuParams" :paramList="paramList"></sju-params> -->
	</view>
</template>

<script setup>
	import {ref} from "vue"
	import {onLoad} from '@dcloudio/uni-app'
	let productInfo=ref([])
	let selectedSku=ref()
	let hei=ref("70vh")
	let flas=ref(false)
	let goodsType=ref('ordinary')
	let productId=ref([])
	let pl=ref("")
	onLoad((op)=>{
		console.log(op.id);
		uni.request({
			// method:"POST"
			url:`http://192.168.212.25:3232/products?id=${op.id}`,
			data:{
				
			},
			success(r){
				console.log(r.data);
				productInfo.value=r.data
				selectedSku.value=r.data
				productId.value=r.data.productId
				uni.request({
					url:`http://192.168.212.25:3232/productsA?id=${1301645275}&page_index=${1}&type=${-999}`,
					data:{
						
					},
					success(res){
						console.log(res);
						pl.value=res.data
					}
				})
			}
		})
		// goodsType.value=op.id
	})
	let tuihuo=()=>{
	flas.value=true
	console.log(flas.value);
	}
	let ddd=()=>{
		flas.value=false
	}
	
// import uniNavBar from "@/components/uni-nav-bar/uni-nav-bar.vue"
// import sjuService from '@/components/sju-service/sju-service.vue'
// import sjuCoupon from '@/components/sju-coupon/sju-coupon.vue'
// import sjuSpecs from '@/components/sju-specs/sju-specs.vue'
// import sjuParams from '@/components/sju-params/sju-params.vue'
// import { 
// 	productInfo, serviceList, skuList, priceList, 
// 	paramList, evaluate, couponCenterList
// } from '@/mock/mock.js'
// 	export default {
// 		data() {
// 			return {
// 				Theme:'',
// 				current:0,
// 				productCode: '',	//商品编号
// 				productInfo:{},
// 				serviceList:[],
// 				skuList:[],
// 				priceList:[],
// 				paramList:[],
// 				skuStr:'请选择商品规格',
// 				selectedSku: {},
// 				evaluate:{},
// 				couponCenterList:[],
				// goodsType:'ordinary'
// 			}
// 		},
// 		components: {
// 			uniNavBar,
// 			sjuService,
// 			sjuCoupon,
// 			sjuSpecs,
// 			sjuParams
// 		},
// 		onLoad(option) {
// 			console.log(option);
// 			this.productCode = option.id
// 			this.goodsType = option.goodsType || 'ordinary'
// 		},
// 		onShow() {
// 			this.Theme = this.$store.state.Theme
// 			this.productInfo = productInfo
// 			this.serviceList = serviceList
// 			this.skuList = skuList
// 			this.priceList = priceList
// 			this.paramList = paramList
// 			this.evaluate = evaluate
// 			this.couponCenterList = couponCenterList
// 			this.paramText = this.paramList.map(item =>{return item.title}).join(' · ')
// 		},
// 		methods: {
// 			// 过滤用户名称
// 			filters_name(value){
// 				if (!value || value === '') 
// 					return ''
// 				let name = ''
// 				if(value.length>2) {
// 					name = this.sjuTools.strFilter(value,1,-2)
// 				} else {
// 					name = value[0] + '****'
// 				}
// 				return name
// 			},
// 			// 轮播图切换
// 			swiperChange(e){
// 				this.current = e.detail.current
// 			},
// 			// 页面跳转
// 			navTo(path){
// 				this.sjuNav.navigateTo(path)
// 			},
// 			// 对象数组属性转数组
// 			objToArray(List){
// 				let arrNew = List.map(item => {
// 				   return item.imgUrl
// 				})
// 				return arrNew
// 			},
// 			// 预览图片
// 			previewImage(urlList, index){
// 				uni.previewImage({
// 					current: index,
// 					urls: urlList
// 				})
// 			},
// 			// 规格选择
// 			onSkuChange(e) {
// 				this.selectedSku = e
// 				if(JSON.stringify(this.selectedSku) !='{}' && this.selectedSku.skuText.length >0) {
// 					let str = ''
// 					this.selectedSku.skuText.forEach((item, index) => {
// 						str += this.skuList[index].name + ':' + item + ' '
// 					})
// 					this.skuStr = str
// 				}
// 			},
// 			// 立即购买
// 			onBuy(e){
// 				let type = {orderType:'goods'}
// 				let item = Object.assign(type,e)
// 				this.sjuNav.navigateTo('/goods/createOrder?item='+JSON.stringify(item))
// 			},
// 			// 加入购物车
// 			onAdd(e){
// 				console.log(e)
// 			},
// 			// 优惠券领取
// 			couponReceive(e) {
// 				console.log(e)
// 			}
			
// 		}
// 	}
</script>

<style lang="scss" scoped>
	.cpmc{
		width: 750rpx;
		margin: auto;
		 .cpmc-cpmc{
			width: 710rpx;
			padding: 15rpx;
			height: 60rpx;
			margin: 20rpx;
			.left{
				width: 200rpx;
				height: 60rpx;
			margin-right: 80rpx;	
			}
			.right{
				// white-space:nowrap\
				width: 500rpx;
				height: 60rpx;
				float: right;
			}
		}
	}
	/* 商品轮播图 */
	.swiper {
		$swiper-width: 100vw;
		// max-width: 750upx;
		$swiper-height: 750upx;
		width: $swiper-width;
		height: $swiper-height;
		swiper-item image{
			width: $swiper-width;
			height: $swiper-height;
		}
		// 轮播页标
		.laber {
			width: fit-content;
			height: 40upx;
			line-height: 40upx;
			text-align: center;
			padding: 0 15upx;
			font-size: 20upx;
			position: absolute;
			right: 15upx;
			bottom: 20upx;
			color: #FFFFFF;
			border-radius: 20upx;
			background-color: rgba(0, 0, 0, 0.4);
		}
	}
	
	// 商品信息
	.goods-info{
		margin: 20upx;
		padding: 0upx 20upx;
		border-radius: $border-radius;
		background-color: var(--accent-bg-color);
		.goods-price{
			padding: 20upx 0;
			// display: flex;
			align-items: center;
			// 商品售价
			.present-price{
				color: $price-color;
				font-size: $font-size-md;
				font-weight: $font-weight-md;
				text{
					font-size: $price-font-size;
				}
				.shou-chang{
					float: right;
					color: #000000;
					line-height: 50rpx;
				}
				.shou-chang image{
					width:50rpx ;
					height: 50rpx;
				}
			}
			// 商品标价
			.original-price{
			max-width: 4.78rem;
			line-height: 54rpx;
			white-space: nowrap;
			overflow: hidden;
			text-overflow: ellipsis;
			font-size: .26rem;
			color: #bf9e6b;
				// text-decoration: line-through;
			}
		}
		// 商品名称
		.goods-name{
			color: #222222;
			font-size: $font-size-lg;
			font-weight: $font-weight-md;
		}
		// 销售量、库存量
		.goods-amount{
			padding: 20upx 0;
			color: #999999;
			font-size: $font-size-sm;
			display: flex;
			align-items: center;
			justify-content: space-between;
			
		}

	}
	.baozheng{
		// display: flex;
		display: block;
		overflow: hidden;
		// float: left;
		
		width: 96vw;
		font-size: 28rpx;
		text-align: center;
		.bao-zzz{
			float: left;
			margin: 0;
			// width: 240rpx;
			.bao-ttt{
				width: 20%;
				// width: 80rpx;
			}
		}
		 .bao-zzz image{
				width: 30rpx;
				height: 30rpx;
				float: left;
				
		}
	}
	.dibu{
		position: fixed;
		bottom: 10vw;
		height: 70vh;
		width: 100vw;
		overflow: hidden;
		.dibu .fuwu{
			width: 100vw !important;
			// width: 100vw !important;
			height: 60% !important;
		}
		.dibu .fuwuqd{
			color: #0055ff;
			background-color:red !important;
		}
	}
	
	// 属性规格
	.tool-box {
		margin: 20upx;
		padding: 0upx 20upx;
		background-color: #FFFFFF;
		border-radius: $border-radius;
		.tool-item{
			padding: 20upx 0;
			display: flex;
			align-items: center;
			justify-content: space-between;
			border-bottom: 1upx solid var(--border-color);
			.item-left{
				width: calc(100% - 120upx);
				flex-shrink: 0;
				color: #999999;
				display: flex;
				align-items: center;
				font-size: $font-size-md;
				
				.item-title{
					color: #999999;
					flex-shrink: 0;
					font-size: $font-size-md;
				}
				
				.item-center{
					width: 100%;
					color: #555555;
					font-size: 28upx;
					flex-shrink: 0;
					margin-left: 20upx;
					word-break: break-all;
					overflow: hidden;
					text-overflow: ellipsis;
					display: -webkit-box;
					-webkit-box-orient: vertical;
					-webkit-line-clamp: 1;
				}
				
				.item-coupon{
					width: calc(100% - 60upx);
					// flex-shrink: 0;
					margin-left: 20upx;
					display: flex;
					align-items: center;
					flex-direction: row;
					-o-text-overflow: ellipsis;
					text-overflow: ellipsis;
					overflow: hidden;
					white-space: nowrap;
					
					.coupon-box{
						flex-shrink: 0;
						padding: 10upx;
						margin-right: 10upx;
						border-radius: 10upx;
						color: var(--primary-color);
						background-color: var(--accent-color);
					}
				}
			}
			
			.iconfont{
				color: #C0C0C0;
			}
		}
	}

	// 评论模块
	.comment{
		width: calc(94% - 40upx);
		padding: 20upx;
		margin: 20upx auto;
		background-color: var(--accent-bg-color);
		border-radius: $border-radius;
		.title-box{
			height: 80upx;
			line-height: 80upx;
			display: flex;
			flex-direction: row;
			align-items: center;
			justify-content: space-between;
			background-color: var(--accent-bg-color);
			
			.title{
				font-size: $font-size-md;
				font-weight: $font-weight-lg;
				text{
					font-size: $font-size-xs;
					margin-left: 8upx;
				}
			}
			
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
			// 右侧内容
			.icon-box{
				height: 80upx;
				line-height: 80upx;
				display: flex;
				align-items: center;
				.icon-name{
					height: 80upx;
					line-height: 80upx;
					color: var(--primary-color);
					// color: #999999;
					font-size: 30upx;
				}
				.iconfont{
					// color: #999999;
					font-size: 40upx;
					color: var(--primary-color);
				}
			}
			
		}
		// 评论列表
		.comment-list{
			padding: 20upx;
			background-color: var(--accent-bg-color);
			border-bottom: 1upx solid #EEEEEE;
			// 用户信息
			.item-user{
				display: flex;
				align-items: center;
				// 用户头像
				.user-image{
					width: 60upx;
					height: 60upx;
					border-radius: 50%;
				}
				.user-info{
					width: calc(100% - 72upx);
					margin-left: 12upx;
					
					.user-name{
						color: #000000;
						font-size: $font-size-lg;
					}
					.date{
						color: #999999;
						font-size: 20upx;
					}
				}
			}
			// 评价内容
			.comment-content{
				margin: 16upx auto;
				color: #000000;
				font-size: $font-size-base;
			}
			
			// 评价图片
			.comment-images{
				width: 100%;
				display: flex;
				flex-wrap: wrap;
				flex-direction: row;
				justify-content: flex-start;
				image{
					width: 200upx;
					height: 200upx;
					margin:0 2% 20upx 0;
					border-radius: 20upx;
				}
				image:nth-child(3n){
					margin-right: 0;
				}
			}
		}
		// 选择最后一个元素
		.comment-list:last-child{
			border-bottom: none;
		}
	}
	
	/* 商品详情 */
	.products-introduction{
		width: 750upx;
		height: auto;
		margin-bottom: 120upx;
		.title{
			display: flex;
			justify-content: center;
			align-items: center;
			width: 100%;
			height: 80upx;
			text{
				font-size: 28upx;
				color: #212121;
				margin: 0 20upx;
			}
		}
		.title:before{
			content: "";
			width: 100upx;
			height: 2upx;
			background-color:#EEEEEE;
		}
		.title:after{
			content: "";
			width: 100upx;
			height: 2upx;
			background-color: #EEEEEE;
		}
		// 商品详情
		.product-content{
			margin-bottom: 20upx;
			::v-deep {
			  img {
			    overflow: hidden;
			    width: 750upx;
			    height: auto;
			  }
			  p {
			    margin: 0;
			    padding: 0;
			    font-size: 0;
			  }
			}
		}
	}
	
	// 商品导航
	.goods-nav{
		z-index: 100;
		width: calc(100% - 40upx);
		padding: 0 20upx;
		height: 100upx;
		position: fixed;
		left: 0;
		bottom: 0;
		display: flex;
		align-items: center;
		justify-content: space-between;
		background-color: #FFFFFF;
		box-shadow: 0 0 10upx 0 rgb(0 0 0 / 10%);
		
		// 导航左侧
		.nav-left{
			width: 40%;
			display: flex;
			align-items: center;
			margin-right: 20upx;
			.item-box{
				flex: 1;
				display: flex;
				align-items: center;
				flex-direction: column;
				justify-content: center;
				font-size: 24upx;
				color: #606266;
				.iconfont{
					font-size: 50upx;
				}
			}
		}
		// 导航右侧
		.nav-right{
			width: 60%;
			text-align: center;
			// color: #FFFFFF;
			font-size: 30upx;
			display: flex;
			align-items: center;
			justify-content: space-between;
			.add-btn{
				width: 49%;
				height: 80upx;
				line-height: 80upx;
				color: var(--primary-color);
				border-top-left-radius: 40upx;
				border-bottom-left-radius: 40upx;
				background-color: var(--accent-color);
				// background: linear-gradient(90deg, #FFCD1E, #FF8A18);
			}
			.buy-btn{
				width: 49%;
				height: 80upx;
				line-height: 80upx;
				// color: #FFFFFF;
				border-top-right-radius: 40upx;
				border-bottom-right-radius: 40upx;
				background: linear-gradient(90deg, var(--primary-color), var(--assist-color));
				// background: linear-gradient(90deg, #FE6035, #EF1224);
			}
			
			.seckill-btn{
				width: 100%;
				height: 80upx;
				line-height: 80upx;
				// color: #FFFFFF;
				border-radius: 40upx;
				background: linear-gradient(90deg, var(--primary-color), var(--assist-color));
			}
		}
	}
	
</style>
