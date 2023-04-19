<template>
	<view class="BOX" v-if="typeList.length!=0">
		<scroll-view :scroll-y="true" class="listBox">
			<view class="listBtn" v-for="(item,i) in typeList" @click="select(i)" :key="i">
					<view :class="i==ins?'cols' : 'colorBox'"></view>
					<view :class="i==ins? 'textB' :'textBox'">{{item.categoryName}}</view>
			</view>
		</scroll-view>
		
		<scroll-view :scroll-y="true" class="shopBox" >
				<view class="shopTop">
						<view class="v1"></view>
						<view class="text">{{typeList[ins].categoryName}}</view>
						<view class="v2"></view>
				</view>
				<view class="shops">
						<view class="clothesBox" v-for="(res,ins) in typeList[ins].subCategoryList" :key="ins*0.1">
								<view class="clothesTop">
										<view class="title">{{res.categoryName}}</view>
										<view touchcancel="gg">更多</view>
								</view>
								<view class="clothes">
										<view class="commodityBox" v-for="(r,j) in res.subCategoryList">
											<img :src="r.imageUrl" alt="">
											<view class="text">{{r.categoryName}}</view>
										</view>
								</view>
						</view>
				</view>
		</scroll-view>
	</view>
</template>

<script setup>
	import uniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue'
	// import { typeList } from '@/mock/mock.js'
	import { ref } from "vue"
	import { onLoad ,onShow} from '@dcloudio/uni-app'
	let typeList = ref([])
	let ins = ref(0)
	onLoad(()=>{
		uni.request({
			url:"http://192.168.212.51:3232/type",
			success(r){
				console.log(r)
				typeList.value = r.data
			}
		})
	})
	
	let select = (i)=>{
		ins.value = i
	}
	// let Theme = ref("")
	// let contentHeight = ref("")
	
	// let typeCode = ref(1)
	// export default {
	// 	data() {
	// 		return {
	// 			Theme:'',
	// 			// 内容高度
	// 			contentHeight: '',
	// 			// 商品类型
	// 			typeList:[],
	// 			typeCode:1,
				
	// 		}
	// 	},
	// 	components: {
	// 		uniNavBar
	// 	},
	// 	onLoad() {
			
	// 	},
		// onShow(()=>{
		// 	// this.Theme = this.$store.state.Theme
		// 	// 获取界面内容高度
		// 	this.contentHeight = (uni.getSystemInfoSync().screenHeight - uni.upx2px(188)) + 'px';
		// 	this.typeList = typeList
			
		// 	//设置 tabBar 选择颜色改为主题色
		// 	uni.setTabBarStyle({
		// 		selectedColor: this.Theme.split(/\;|\:/)[1],
		// 	})
		// 	// 设置tabBar图标
		// 	if(this.sjuLogin.getValue('themeName') != '') {
		// 		this.sjuTools.setTabBar()
		// 	}
		// })
	// 	methods: {
	// 		// 选择商品分类
	// 		changeType(code){
	// 			// 切换选择
	// 			this.typeCode = code
	// 		}
	// 	}
	// }
</script>

<!-- <style>
	page{
		padding-bottom: 0;
	}
</style> -->

<style lang="scss" scoped>
		.BOX{
			width: 100vw;
			height: 91vh;
			display: flex;
			justify-content: space-between;
		}
		.listBox{
			width: 25vw;
			height: 91vh;
		}
		.listBtn{
			display: flex;
			align-items: center;
			width: 25vw;
			height: 4vh;
			margin-bottom: 2vh;
		}
		.listBtn .colorBox{
			width: 1vw;
			height: 3vh;
			// background-color: red;
		}
		.listBtn .textBox{
			width: 24vw;
			text-align: center;
		}
		.listBtn .cols{
			width: 1vw;
			height: 3vh;
			background-color:#7f4395;
		}
		.listBtn .textB{
			width: 24vw;
			text-align: center;
			color: #7f4395;
		}
		.shopBox{
			width: 75vw;
			height: 91vh;
		}
		.shopTop{
			width: 100%;
			height: 5vh;
			display: flex;
			justify-content: center;
		}
		.shopTop .v1{
			width: 10vw;
			height: 100%;
			background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAACCAYAAAAesF8hAAAAAXNSR0IArs4c6QAAAJhJREFUGBldT0sKRCEM8/P6wIX3v4N3mGOpC1EnKVORKUjTNInqSymx1iprLem9v957SSnJGON1zgnPnFOxoIj33g90bwjhaKhDhs7gBZqzM2y8+eE5Gvr/deRunm+j5/J/HhCncs4Oj3CtNQfxjjHqDn3j8oPxCc8B2sNzNg0zUBsZ7Jp1Y1K6gObXtd1+3m+7G4NjrvnXF/XOUa7X8vi3AAAAAElFTkSuQmCC) no-repeat center center;
		}
		
		.shopTop .text{
			height: 100%;
			line-height: 5vh;
			margin: 0 3vw;
			font-size: 28rpx;
		}
		.shopTop .v2{
			width: 10vw;
			height: 100%;
			background: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAADAAAAACCAYAAAAesF8hAAAAAXNSR0IArs4c6QAAAKBJREFUGBl1TsENxDAIS9JGatSxbsrucbNkk35KEmpzJernkBDG2MjxOI5vCOGDvtDtmaKqAiwxRvLcbb454qcv6qBprh9j/PWnlOYv+qG9yGG2ZVnsJijefG+teQ4ppVAv+75LgsgrOkCQgCDKBmcT2O7kUK7l3erFBQSZfhP/fk0/7oYxp985PmN2BFd2711zzrquq3m2bdPzPA3XWvUGPwyDhD95AlIAAAAASUVORK5CYII=) no-repeat center center;
		}
		.shopBox .shops{
			width: 100%;
		}
		.clothesBox{
			width: 100%;
		}
		.clothesBox .clothesTop{
			width: 100%;
			height: 5vh;
			display: flex;
			justify-content: space-between;
			align-items: center;
		}
		.clothesBox .clothes{
			width: 100%;
			display: flex;
			flex-wrap: wrap;
		}
		.clothesBox .clothes .commodityBox{
			width: 21vw;
			// height: 18vh;
			padding: 1vh 2vw;
			margin-bottom: 1vh;
		}
		.clothesBox .clothes .commodityBox img{
			width: 21vw;
		}
		.clothesBox .clothes .commodityBox .text{
			width: 21vw;
			font-size: 12rpx;
			text-align: center;
		}
</style>
