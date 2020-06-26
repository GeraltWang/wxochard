<template>
	<view>
		<view class="toptext-box">
			<text>商品清单</text>
			<text>总重约{{weight}}KG</text>
		</view>
		<scroll-view scroll-y="true" class="scroll-list">
			<view v-for="(item,index) in data" :key=index>
				<van-card :price="item.price" :desc=item.volume :title=item.title :thumb="'http://192.168.123.204:8088/'+item.image">

					<view slot="footer">
						<van-checkbox class='pro-check' checked-color="#07c160"></van-checkbox>
						<van-button size="mini" type="primary">-</van-button>
						<text>2</text>
						<van-button size="mini" type="info">+</van-button>
					</view>
				</van-card>
			</view>
		</scroll-view>
		<van-submit-bar :price="3050" button-text="提交订单" @submit="onSubmit">
			<van-checkbox>全选</van-checkbox>
		</van-submit-bar>
	</view>
</template>

<script>
	export default {
		created() {
			this.loaddata()

		},
		data() {
			return {
				data: '',
				weight: 0,
				allchecked: true
			};
		},
		methods: {
			loaddata() {
				uni.getStorage({
					key: 'cartstorage',
					success(res) {
						this.data = res.data
					}
				})
				// this.data = currentCart
				console.log(this.data)
			},
			// selected(item) {
				
			// 	item.flag = !item.flag
			// 	if (!item.flag) {
			// 		this.allchecked = false
			// 	} else {
			// 		const test = this.myData.every(item => {
			// 			return item.flag === true
			// 		})
			// 		if (test) {
			// 			this.allchecked = true
			// 		} else {
			// 			this.allchecked = false
			// 		}
			// 	}

			// },
			
			// selectedall() {
			// 	this.allchecked = !this.allchecked
			// 	if (this.allchecked) {
			// 		this.myData.map(item => {
			// 			item.flag = true
			// 		})
			// 	} else {
			// 		this.myData.map(item => {
			// 			item.flag = false
			// 		})
			// 	}
			// },
		}
	}
</script>

<style lang="less">
	.toptext-box {
		width: 100%;
		display: flex;
		justify-content: space-between;
		position: fixed;
		top: 0;
		left: 0;
	}

	.scroll-list {
		height: 1150rpx;
		// margin-bottom: 130rpx;
		position: relative;
		top: 50rpx;
		// margin-top: 100rpx;
	}

	.pro-check {
		position: relative;
		top: 30rpx;
	}

	.btn {
		width: 50rpx;
		height: 50rpx;
	}
</style>
