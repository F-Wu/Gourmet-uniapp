<template>
	<view class="container">
		<u-search :show-action="false" :placeholder="search_text" class="search" margin="15rpx" bg-color="#f6f6f6"
			@search="Searchfood(food)" v-model="food">
		</u-search>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				search_text: "请输入关键字进行搜索",
				food: '',
				// foods:[]
			}
		},
		methods: {
			Searchfood(food) {
				uni.navigateTo({
					url:"../../pages/foodList/foodList?food="+this.food
				})
			}
		}
	}
</script>

<style>
.search{
	/* border: 1rpx solid #808080; */
}
</style>
