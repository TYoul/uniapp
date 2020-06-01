<template>
	<view>
		<ShopList :keyword="keyword"></ShopList>
	</view>
</template>

<script>
import ShopList from '@/components/common/ShopList.vue'

export default {
	data() {
		return {
			keyword:''
		}
	},
	onLoad(e){
		// #ifdef APP-PLUS  
		var webView = this.$mp.page.$getAppWebview();  
		// 设置 searchInput的 text  
		webView.setTitleNViewSearchInputText(e.keyword);  
		// #endif
		this.keyword = e.keyword
	},
	methods: {
		
	},
	components:{
		ShopList
	}
}
</script>

<style>

</style>
