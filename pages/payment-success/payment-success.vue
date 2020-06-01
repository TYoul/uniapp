<template>
	<view class="payment-success">
		<view class="f-active-color" style="font-size: 52rpx;">恭喜您支付成功</view>
		<view @tap='goIndex' class="f-active-color return-index">返回首页</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				
			}
		},
		methods: {
			goIndex(){
				uni.switchTab({
					url:'/pages/index/index'
				})
			}
		}
	}
</script>

<style scoped>
.payment-success{
	position: absolute;
	left: 0;
	right: 0;
	bottom: 0;
	top: 0;
	display: flex;
	justify-content: center;
	flex-direction: column;
	align-items: center;
}
.return-index{
	padding: 10rpx 60rpx;
	border: 2px solid #49BDFB;
	border-radius: 40rpx;
}
</style>
