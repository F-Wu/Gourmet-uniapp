<template>
	<view>
		<Search></Search>
		<view class="fasten">
			<!-- <u-tag :text="num" shape="circle"></u-tag> -->
			<!-- <u-number-box class="pagination"></u-number-box> -->
			<uni-pagination showAround btnText showPageInfo :total="page.total" @change="changePage">
			</uni-pagination>
		</view>
		<ul style="margin-bottom: 30px;">
			<li @click="detailed(item.id)" v-for="(item,index) in foods" :class="[{'Leftmove':index%2 != 1},{'Reightmove':index%2 != 0}]">
				<image :src="item.thumb" class="img"></image>
				<view class="content">
					<p>{{item.title}}</p>
					<!-- <p>适宜人数:<text>{{item.peoplenum}}</text></p>
					<p>烹饪时长:<text>{{item.cookingtime}}</text></p> -->
					<span v-for="(tage,index1) in item.cid.slice(0,item.cid.length-1).split(',')">
						<u-tag :text="tage" shape="circle" size="mini" class="tag" />
					</span>
					<!-- 烹饪时长 -->
					<u-icon name="../../static/Workinghours.png" :label="item.costtime" 
					size="34" label-size="34" style="display: block;" v-if="item.costtime!=null"></u-icon>
					<view class="btm">
						<u-icon name="eye" :label="item.viewnum" size="28"></u-icon>
						<u-icon name="star" :label="item.favnum" size="28" class="end"></u-icon>
					</view>
				</view>
			</li>
		</ul>
	</view>
</template>

<script>
	import Search from '../../components/search/search'
	import uniPagination from '../../node_modules/@dcloudio/uni-ui/lib/uni-pagination/uni-pagination.vue'
	export default {
		data() {
			return {
				keyword: null,
				classid: null,
				foods: [],
				page: {
					total: '',
					pageSize: 10,
					currentPage: ''
				},
				tages: [],
				type: [],
				id: ''
			}
		},
		methods: {
			getData() {
				uni.request({
					method: 'GET',
					// url: 'http://192.168.43.84:8081/Gourmet/api/search',
					url:'http://localhost:8081/Gourmet/api/search',
					data: {
						// appkey: "b1885f929618d79f",
						keyword: this.keyword,
						// num: parseInt(this.page.pageSize)
						num: this.page.currentPage
					}
				}).then(res => {
					let info = res[1].data
					if (info.status == 0) {
						this.foods = info.result;
						this.page.total = info.total
					}else {
						console.log(info.msg)
					}
				})
			},
			getDataTow() {
				uni.request({
					method: 'GET',
					// url: 'http://192.168.43.84:8081/Gourmet/api/byclass',
					url:'http://localhost:8081/Gourmet/api/byclass',
					data: {
						id: this.id,
						num: this.page.currentPage
					}
				}).then(res => {
					let info = res[1].data
					if (info.status == 0) {
						this.foods = info.result;
						this.page.total = info.total
					}else {
						console.log(info.msg)
					}
				})
			},
			// 页码变动
			changePage(params) {
				this.page.currentPage = params.current + "0"
				console.log(parseInt(this.page.currentPage))
				this.getData()
				// if (this.page.currentPage > 10) {
				// 	this.foods = this.foods.slice(parseInt(this.page.currentPage) - 10, 10)
				// }
			},
			// 详细
			detailed(id) {
				uni.navigateTo({
					url: "../Detailed/Detailed?id=" + id
				})
			},
		},
		components: {
			Search,
			uniPagination
		},
		onLoad(option) {
			this.keyword = option.food
			this.id = option.id
			console.log("keyword-----" + this.keyword)
			console.log("id-----" + this.id)
			if (this.id == undefined) {
				this.getData()
			} else {
				this.getDataTow()
			}
		}
	}
</script>

<style>
	.fasten {
		/* display: grid; */
		/* margin-left: 20px; */
		/* width: 100%; */
		margin: 0 auto;
	}

	.pagination {
		margin-left: 20px;
	}

	ul {
		margin: 0 auto;
		/* background-color: #007AFF; */
	}

	li {
		width: 90%;
		list-style: none;
		box-shadow: 0px 2px 2px 3px #cecece;
		/* animation: 0.8s linear Move; */
		padding: 5px 0px;
		display: flex;
	}

	.Leftmove {
		animation: 0.8s linear LeftMove;
	}

	.Reightmove {
		animation: 0.8s linear reightMove;
	}

	@keyframes LeftMove {
		from {
			margin-left: 200px;
		}

		to {
			margin-left: 0px;
		}
	}

	@keyframes reightMove {
		from {
			margin-left: -200px;
		}

		to {
			margin-left: 0px;
		}
	}

	.content {
		width: 60%;
		height: 110px;
		overflow: hidden;
		font-family: "宋体";
		padding-left: 5px;
		top: 5px;
		position: relative;
	}

	.content p {
		font-weight: 700;
	}

	.tag {
		margin-top: 5px;
	}

	.flex {
		display: flex;
	}

	.item-title {
		font-size: 26rpx;
		color: $u-main-color;
		font-weight: bold;
	}

	.img {
		width: 40%;
		height: 110px;
	}

	.smlimg {
		width: 18px;
		height: 18px;
	}

	.tag {
		margin-left: 3px;
	}

	.btm {
		position: absolute;
		bottom: 10px;
		width: 100%;
	}

	.end {
		position: absolute;
		right: 10px;
	}
</style>
