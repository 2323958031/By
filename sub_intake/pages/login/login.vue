<template>
	<view :style="Theme">
		<uni-nav-bar leftIcon="arrowleft" fixed="true" title="登录" />
		<view class="logo">
			<image src="/static/logo.png"></image>
		</view>
		<view class="loginForm">
			<view class="row-input">
				<i class="iconfont icon-shouji" />
				<input placeholder="请输入手机号" @input="in1($event)" :value="vau" maxlength="11" placeholder-class="placeholder" />
			</view>
			<view class="row-input">
				<i class="iconfont icon-mima" />
				<input placeholder="请输入密码" @input="in2($event)" :value="vau1" maxlength="18" password placeholder-class="placeholder" />
			</view>
			
			<button style="background-color:#279b37;color: #fff;" @click="login">立即登录</button>
			<view class="menuLink">
				<text @click="navTo('/intake/register')">注册账号</text>
				<text @click="navTo('/intake/forget')">忘记密码</text>
			</view>
		</view>
	</view>
</template>

<script setup>
	import uniNavBar from '@/components/uni-nav-bar/uni-nav-bar.vue'
	import { onLoad } from '@dcloudio/uni-app'
	import axios from 'axios'
	import { ref }  from "vue"
	onLoad(()=>{
		console.log('创建')
		// uni.request({
		//         url: '/app/api/rmsp',
		//         method: 'GET',
		// 		success(r){
		// 			console.log(r)
		// 		}
		//       })
		axios.get("/api/rmsp")
		.then(res=>{
			console.log(res)
		})
	})
	let Theme = ref("")
	let vau = ref("")
	let vau1 = ref("")
	let in1 = function(e){
		vau.value = e.detail.value
		console.log(vau.value)
	}
	
	let in2 = function(e){
		vau1.value = e.detail.value
		console.log(vau1.value,"1")
	}
	
	let login = function(){
		
	}
	
	// uni.request(
	// 	url:`/apirmsp`,
	// 	success(r){
	// 		console.log(r)
	// 	}
	// )
	
	
</script>

<style lang="scss">
	.logo{
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 300upx;
		image{
			width: 200upx;
			height: 200upx;
		}
	}
	
	.loginForm{
		padding: 20rpx 60rpx;
		
		.row-input {
			margin: 30rpx 0rpx;
			display: flex;
			justify-content: flex-start;
			align-items: center;
			padding: 0 20rpx;
			height: 100rpx;
			border: 1rpx solid var(--primary-color);
			border-radius: 20rpx;
		
			image {
				padding: 0rpx 10rpx 0rpx 10rpx;
				width: 50rpx;
				height: 50rpx;
				flex-shrink: 0;
			}
			
			.iconfont {
				color: var(--primary-color);
				padding: 0rpx 10rpx 0rpx 10rpx;
				flex-shrink: 0;
				font-size: 40upx;
			}
			
			.placeholder {
				font-size: 30rpx;
				color: var(--primary-color);
			}
		
			input {
				font-size: 30rpx;
				padding: 0rpx 40rpx;
				color: var(--primary-color);
				border-left: 3rpx solid var(--primary-color);
			}
		}
		
		.loginBtn {
			display: flex;
			justify-content: center;
			align-items: center;
			font-size: 35rpx;
			height: 100rpx;
			border-radius: 20rpx;
			color: #FFFFFF;
			background-color: var(--btn-bg-color);
		}
		
		.menuLink {
			display: flex;
			justify-content: space-between;
			font-size: 28rpx;
			color: var(--btn-bg-color);
			letter-spacing: 3rpx;
			line-height: 80rpx;
		}
	}
	
	
	
</style>
