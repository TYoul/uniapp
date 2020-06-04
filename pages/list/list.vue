<template>
	<view>
		<Lines></Lines>
		<view class="list">
			<!-- 左侧滑动 -->
			<scroll-view scroll-y="true" class="list-left" :style="'height:'+clentHeight+'px;'">
				<view v-for="(item,index) in leftData" 
					  :key="index"
					  class="left-item" 
					  @tap="changeLeftTab(index,item.id)">
					<view class="left-name"
						  :class="activeIndex===index?'left-name-active':''">{{item.name}}</view>
				</view>
			</scroll-view>
			
			<!-- 右侧滑动 -->
			<scroll-view scroll-y="true" class="list-right" :style="'height:'+clentHeight+'px;'">
				<view class="right-list" 
					  v-for="(item,index) in rightData"
					  :key="index">
					<block v-for="(k,i) in item" :key="i">
						<view class="list-title">{{k.name}}</view>
						<view class="right-content">
							<view class="right-item" v-for="(j,idx) in k.list" :key="idx">
								<image class="right-img" :src="j.imgUrl" mode=""></image>
								<view class="right-name">{{j.name}}</view>
							</view>
						</view>
					</block>
				</view>				
			</scroll-view>
		</view>
		<Tabbar currentPage='list'></Tabbar>
	</view>
</template>

<script>
import $http from "@/common/api/request.js"
import Lines from '@/components/common/Lines.vue'
import Tabbar from '@/components/common/Tabbar.vue'
	export default {
		data() {
			return {
				clentHeight:0,
				activeIndex:0,
				leftData:[],
				rightData:[]
			}
		},
		//获取可视高度
		onReady(){
			// let view = uni.createSelectorQuery().select(".home-data");
			// view.boundingClientRect(data => {
			//   this.clentHeight = 10000
			//   // data.height
			// }).exec();
			
			uni.getSystemInfo({
				success: (res) => {
					this.clentHeight = res.windowHeight;
				}
			})
		},
		components:{
			Lines,
			Tabbar
		},
		//input输入框点击事件
		onNavigationBarSearchInputClicked(){
			uni.navigateTo({
				url:'/pages/search/search'
			})
		},
		onLoad() {
			this.getData()
		},
		methods: {
			//请求数据方法
			getData(id){
				if(id === (this.activeIndex+1)){
					return;
				}
				
				$http.request({
					url:"/goods/list"
				}).then((res)=>{
					let leftData = [];
					let rightData = []
					res.forEach(v=>{
						leftData.push({
							id:v.id,
							name:v.name
						})
						//如果点击的id值相同
						if(v.id === (this.activeIndex+1)){
							rightData.push(v.data)
						}
					})
					this.leftData = leftData
					this.rightData = rightData
				}).catch(()=>{
					uni.showToast({
						title:'请求失败',
						icon:'none'
					})
				})
			},
			//左侧点击事件
			changeLeftTab(index,id){
				this.getData(id);
				this.activeIndex = index;
			}
		}
	}
</script>

<style scoped>
.list{
	display: flex;
}
.left-item{
	border-bottom: 2rpx solid #FFFFFF;
	font-size: 28rpx;
	font-weight: bold;
	background-color: #F7F7F7
}
.left-name{
	padding: 30rpx 6rpx;
	text-align: center;
}
.left-name-active{
	border-left: 8rpx solid #49BDFB;
	background-color: #FFFFFF;
}
.list-left{
	width: 200rpx;
}
.list-right{
	flex: 1;
	padding-left: 30rpx;
}
.list-title{
	font-weight: bold;
	padding: 30rpx 0;
}
.right-content{
	display: flex;
	flex-wrap: wrap;
}
.right-name{
	padding: 16rpx 0;
}
.right-img{
	width: 150rpx;
	height: 150rpx;
}
.right-item{
	width: 150rpx;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	padding: 10rpx;
}
</style>
