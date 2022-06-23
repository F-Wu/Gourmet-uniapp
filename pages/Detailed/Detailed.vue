<template>
	<view class="wrap">
		<view class="subject">
			<!-- img -->
			<image :src="foods.thumb" class="img" />
			<!-- name -->
			<p>{{foods.title}}</p>
			<!-- tag -->
			<span v-for="(tage,index) in tages.split(',\n')">
				<u-tag :text="tage" shape="circle" size="mini" class="tag" />
			</span>
		</view>
		<view>
			<!--主料  type=1 -->
			<view class="Ingredients">
				<p>材料</p>
				<scroll-view :scroll-x="true" :scroll-with-animation="true" class="categorys">
					<table>
						<tr>
							<th class="th" v-for="(item,index) in yl.split('#\n')">{{item}}</th>
						</tr>
						<tr>
							<th class="th" v-for="(item,index) in fl.split('#\n')">{{item}}</th>
						</tr>
					</table>
				</scroll-view>
			</view>

			<!-- 步骤 -->
			<view class="step">
				<p>步骤</p>

				<view class="container">
					<view v-for="(item1,index) in foods.steppic.split('#\n')">
						<view class="title">
							<view class="line"></view>
							<span>第{{index+1}}步</span>
						</view>
						<image :src="item1" class="img" />
						<p class="pcontent">{{foods.steptext.slice(0,foods.steptext.length-1).split('#\n')[index]}}</p>
					</view>
				</view>
			</view>
		</view>

	</view>

</template>

<script>
	export default {
		data() {
			return {
				foodId: null,
				foods: {},
				tages: null,
				yl: null,
				fl: null
			}
		},
		methods: {
			getData() {
				uni.request({
					method: 'GET',
					// url: 'http://192.168.43.84:8081/Gourmet/api/detail',
					url:'http://localhost:8081/Gourmet/api/detail',
					data: {
						appkey: "b1885f929618d79f",
						id: this.foodId,
					}
				}).then(res => {
					let info = res[1].data
					if (info.status == 0) {
						this.foods = info.result[0];
						this.tages = info.result[0].cid.slice(0, this.foods.cid.length - 1)
						this.fl = info.result[0].fl.slice(0, this.foods.fl.length - 1)
						this.yl = info.result[0].yl.slice(0, this.foods.yl.length - 1)
						console.log(this.yl)
					} else {
						console.log(info.msg)
					}
				})
			},
		},
		onLoad(option) {
			this.foodId = option.id
			console.log(option.id)
			this.getData()
		},
		// computed: {
		// 	listIngredients() {
		// 		return this.foods.material.filter(function(data) {
		// 			//只返回id是偶数的
		// 			return parseInt(data.type)==1
		// 		})
		// 	},
		// 	listAccessories(){
		// 		return this.foods.material.filter(function(data) {
		// 			//只返回id是偶数的
		// 			return parseInt(data.type)==0
		// 		})
		// 	}
		// }
	}
</script>

<style>
	.wrap {
		background-color: #f6f6f6;
		/* margin: 0 auto; */
	}

	.subject,
	.Ingredients>p,
	.accessories>p,
	.step>p {
		text-align: center;
		font-size: 1.2em;
		font-weight: 600;
		background-color: #FFFFFF;
	}

	.subject p {
		margin: 7px 0;
	}

	.categorys {
		margin: 20rpx 0;
		font-size: 26rpx;
		text-align: center;
		white-space: nowrap;
		display: flex;
		align-items: center;
		justify-content: center;
		background-color: #FFFFFF;
		padding: 20rpx 0;
	}

	.th {
		background-color: #A0CFFF;
		width: 200px !important;
		padding: 00 10px;
	}

	.tag {
		margin: 0 3px;
		margin-bottom: 10px;
	}

	.img {
		width: 100%;
		text-align: center;
	}

	.Ingredients,
	.accessories,
	.step {
		margin-top: 10px;
		padding: 5px;
		background-color: #FFFFFF;
	}

	ul {
		width: 100%;
		/* background-color: #55aaff; */
		text-align: left;
		list-style: none;
		font-size: 1.1em;
	}

	.title {
		font-size: 1.1em;
		font-weight: 700;
		color: #F0AD4E;
		/* height: 16px; */
		/* background-color: #007AFF; */
	}

	.title>span {
		line-height: 18px;
	}

	.line {
		width: 5px;
		height: 18px;
		background-color: #f0d239;
		margin-right: 8px;
		display: inline-block;
		/* 	width: 16px;
		height: 16px; */
	}

	.pcontent {
		text-align: center;
		font-size: 1.1em;
	}

	.container {
		margin: 10px 0;
	}

	.flex {
		display: flex;
	}
</style>
