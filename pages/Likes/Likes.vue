<template>
	<view>
		<Search></Search>
		<view class="fasten">
			<uni-pagination showAround btnText showPageInfo :total="page.total" @change="changePage">
			</uni-pagination>
		</view>
		<view class="card">
			<uni-card mode="style" :is-shadow="true" v-for="(item,index) in Likses" 
			:key="index" :title="item.title"
			:thumbnail="item.thumb" :extra="item.create_time" 
			:note="item.zid" @click="detailed(item.id)">
			</uni-card>
		</view>
	</view>
</template>

<script>
	import Search from '../../components/search/search'
	import uniPagination from '../../node_modules/@dcloudio/uni-ui/lib/uni-pagination/uni-pagination.vue'
	import uniCard from '../../node_modules/@dcloudio/uni-ui/lib/uni-card/uni-card.vue'
	export default {
		data() {
			return {
				page: {
					total: '',
					pageSize: 10,
					currentPage: ''
				},
				Likses: null
			}
		},
		methods: {
			Getlike() {
				uni.request({
					method: 'GET',
					 // url: 'http://192.168.43.84:8081/Gourmet/api/like',
					 url:'http://localhost:8081/Gourmet/api/like',
					data: {
						num: this.page.currentPage
					}
				}).then(res => {
					let info = res[1].data
					if (info.status == 0) {
						this.Likses = info.result;
					} else {
						console.log(info.msg)
					}
				})
			},
			clickCard() {
				uni.showToast({
					title: '点击卡片',
					icon: 'none'
				});
			},
			// Getlike() {
			// 	uni.request({
			// 		method: 'GET',
			// 		// url: 'http://192.168.43.84:8081/Gourmet/api/like',
			// 		url:'http://localhost:8081/Gourmet/api/like',
			// 	}).then(res => {
			// 		let info = res[1].data
			// 		if (info.status == 0) {
			// 			this.Likses = info.result;
			// 		} else {
			// 			console.log(info.msg)
			// 		}
			// 	})
			// },
			// 详细
			detailed(id) {
				uni.navigateTo({
					url: "../Detailed/Detailed?id=" + id
				})
			},
		},
		components: {
			Search,
			uniPagination,
			uniCard
		},
		onLoad() {
			this.Getlike()
		}
	}
</script>

<style>
	.fasten {
		margin: 0 auto;
	}

	.flex {
		display: flex;
	}

	.pagination {
		margin-left: 20px;
	}
</style>
