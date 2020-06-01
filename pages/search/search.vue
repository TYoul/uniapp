<template>
	<view class="search">
		<Lines></Lines>
		
		<view class="search-item">
			<view class="search-title">
				<view class="f-color">最近搜索</view>
				<view class="iconfont icon-lajitong"
					  @tap="cleanHistory"></view>
			</view>
		
			<view v-if="searchData.length > 0">
				<view class="search-name f-color"
					  v-for="(item,index) in searchData"
					  :key="index"
					  @tap="toSearchList(item)">
					  {{item}}
				</view>
			</view>
			<view v-else class="search-end">暂无搜索记录</view>
		
		</view>
		
		<view class="search-item">
			<view class="search-title">
				<view class="f-color">热门搜索</view>
			</view>
		
			<view>
				<view class="search-name f-color">四件套</view>
				<view class="search-name f-color">面膜</view>
			</view>
		
		</view>
		
	</view>
</template>

<script>
import Lines from "@/components/common/Lines.vue"
	export default {
		data() {
			return {
				//输入的关键词
				keyword:'',
				//搜索过的词记录
				searchData:[]
			}
		},
		components:{
			Lines
		},
		//页面加载的时候
		onLoad() {
			uni.getStorage({
				key:"searchData",
				success: (res) => {
					// console.log(res)
					this.searchData = JSON.parse(res.data)
				}
			})
		},
		//监听input输入内容
		onNavigationBarSearchInputChanged(e){
			// console.log(e)
			this.keyword = e.text
		},
		//标题栏按钮点击搜索
		onNavigationBarButtonTap(e){
			this.search()
		},
		//监听软键盘搜索按钮点击
		onNavigationBarSearchInputConfirmed(e){
			this.search()
		},
		methods: {
			//判断关键词是否为空和跳转页面的
			search(){
				if(this.keyword === ""){
					return uni.showToast({
						title:"关键词不能为空",
						icon:"none"
					})
				}else{
					this.toSearchList(this.keyword)
				}
				uni.hideKeyboard()
				this.addSearch()
			},
			//记录最近搜索值
			addSearch(){
				let idx = this.searchData.indexOf(this.keyword)
				if( idx === -1){
					this.searchData.unshift(this.keyword)
				}else{
					this.searchData.unshift(...this.searchData.splice(idx,1))
				}
				
				uni.setStorage({
					key:"searchData",
					data:JSON.stringify (this.searchData)
				})
			},
			//清除搜索记录
			cleanHistory(){
				uni.showModal({
					title:'重要提示',
					content:'是否要清除记录',
					cancelText:'取消',
					confirmText:'确定',
					success: (res) => {
						// console.log(res)
						if(res.confirm){
							uni.removeStorage({
								key:"searchData"
							})
							this.searchData = []
						}
					}
				})
			},
			//点击搜索记录进入页面
			toSearchList(item){
				uni.navigateTo({
					url:"/pages/searchList/searchList?keyword="+item+""
				})
			}
		}	
	}
</script>

<style scoped>
.search-item{
	padding: 20rpx;
}
.search-title{
	display: flex;
	/* flex-direction:row ; */
	justify-content: space-between;
}
.search-name{
	padding: 4rpx 24rpx;
	background-color: #E1E1E1;
	display: inline-block;
	margin: 10rpx;
	border-radius: 26rpx;
}
.search-end{
	text-align: center;
}
</style>
