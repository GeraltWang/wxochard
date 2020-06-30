<template>
	<view>
		<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" class="swiper" circular="true">
			<swiper-item v-for="(item, index) in page.templatePhoto" :key="index">
				<view>
					<!-- <image :src="'http://192.168.123.204:8088/'+item" mode="" class="banner-img"></image> -->
					<image :src="'http://192.168.0.167:8088/' + item" mode="" class="banner-img"></image>
				</view>
			</swiper-item>
		</swiper>
		<view class="card">
			<view class="title">{{ pname }}</view>
			<view class="subtitle">{{ page.productInfo.product_desc }}</view>
			<view class="price">￥{{ cartitem.price }}</view>
			<view class="btn">
				<button
					type="default"
					:class="active == index ? 'orangebtn' : ''"
					size="mini"
					v-for="(item, index) in page.productItem"
					:key="index"
					@click="switchItem(item, index)"
				>
					{{ item.volume }}
				</button>
			</view>
			<view class="sendtime">{{ page.sendTimeMsg }}</view>
			<view class="service">
				<text>
					<van-icon name="arrow" color="red" />
					<text>保障</text>
				</text>
				<text>{{ page.refundRule }}</text>
				<van-icon name="arrow" color="black" />
			</view>
		</view>
		<view class="desc">
			<view class="box"><image src="http://192.168.0.167:8088/images/090734.png" mode=""></image></view>
			<view class="info">
				<view class="i-1">{{ page.templateInfo.pro_info.origin }}</view>
				<view class="i-2">{{ page.templateInfo.pro_info.originMsg }}</view>
				<view class="i-3">{{ page.templateInfo.pro_info.explain }}</view>
				<view class="i-4">{{ page.templateInfo.pro_info.explainMsg }}</view>
				<view class="i-5">{{ page.templateInfo.pro_info.store }}</view>
				<view class="i-6">{{ page.templateInfo.pro_info.storeMsg }}</view>
			</view>
			<view class="img"><image :src="'http://192.168.0.167:8088/' + item" mode="widthFix" v-for="(item, index) in page.templateInfo.desc_imgs" :key="item.id"></image></view>
		</view>
		<view class="nav">
			<view class="cartinfo" @click="switchTab">
				<van-icon name="cart-o" :info="cartnum" />
				<text>购物车</text>
			</view>
			<view class=""></view>
			<view class="button-box"><button class="itself" type="default" @click="onadd(cartitem)">加入购物车</button></view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			pname: '',
			purl: '',
			pid: '',
			page: [],
			cartitem: {
				id: '',
				name: '',
				price: 0,
				weight: '',
				url: '',
				sum: 1,
				checked: true
			},
			cartnum: 0,
			indicatorDots: true,
			autoplay: true,
			interval: 3000,
			duration: 500,
			active: 0
		};
	},
	onLoad(option) {
		console.log(option);
		this.pname = option.name;
		this.cartitem.name = option.name;
		this.purl = option.url;
		this.cartitem.url = option.url;
		this.pid = option.id;
		// this.cartitem.id = option.id
		let that = this;
		uni.getStorage({
			key: 'cartstorage',
			success(res) {
				that.cartnum = res.data.length;
			}
		});
		this.request(option);
	},
	methods: {
		request(js) {
			uni.request({
				// url: `http://192.168.123.204:8088/data/good_detail_${js.id}.json`,
				url: `http://192.168.0.167:8088/data/good_detail_${js.id}.json`,
				method: 'GET',
				header: {
					'content-type': 'application/json'
				},
				success: res => {
					// console.log(res);
					this.page = res.data;
					this.cartitem.id = res.data.productItem[0].id;
					this.cartitem.price = res.data.productItem[0].price;
					this.cartitem.weight = res.data.productItem[0].volume;

					console.log(this.page);
					console.log(this.cartitem);
				}
			});
		},
		switchItem(item, index) {
			this.active = index;
			(this.cartitem = {
				id: item.id,
				name: this.pname,
				price: item.price,
				weight: item.volume,
				url: this.purl,
				sum: 1,
				checked: true
			}),
				console.log(this.cartitem);
		},
		onadd(item) {
			console.log(item);
			let that = this;
			uni.getStorage({
				key: 'cartstorage',
				success(res) {
					console.log(res.data);
					var count = 0;
					var dd = res.data;
					console.log(dd);
					for (var i = 0; i < res.data.length; i++) {
						if (dd[i].id == item.id) {
							dd[i].sum++;
							count++;
						}
					}
					if (count == 0) {
						dd.push({
							id: item.id,
							name: item.name,
							price: item.price,
							weight: item.weight,
							url: item.url,
							sum: 1,
							checked: true
						});
					}

					uni.setStorage({
						key: 'cartstorage',
						data: dd,
						success() {
							console.log('success');
						}
					});
				},
				fail(res) {
					// uni.getStorage({
					// 	key: 'cartstorage',
					// 	success(res) {
					// 		that.cartnum = res.data.length;
					// 	}
					// });
					uni.setStorage({
						key: 'cartstorage',
						data: [
							{
								id: item.id,
								name: item.name,
								price: item.price,
								weight: item.weight,
								url: item.url,
								sum: 1,
								checked: true
							}
						],
						success() {
							console.log('success');
						}
					});
				}
			});
		},
		switchTab() {
			uni.switchTab({
				url: '/pages/cart/cart'
			});
		}
	}
};
</script>

<style lang="less" scoped>
.swiper {
	width: 100%;
	height: 290px;
	.banner-img {
		width: 100%;
		height: 600rpx;
	}
}

.card {
	margin: auto;
	width: 90%;
	height: 270px;
	display: flex;
	flex-direction: column;
	justify-content: space-evenly;
	align-items: center;
	border-radius: 30rpx;
	box-sizing: border-box;
	box-shadow: rgba(0, 0, 0, 0.2) 0 1px 5px 0px;
	position: absolute;
	top: 250px;
	left: 18.5px;
	background-color: #ffffff;

	.title {
		font-weight: bold;
	}

	.subtitle {
		color: gray;
	}

	.price {
		color: #ff8000;
		font-size: 22px;
		font-weight: bold;
	}

	.btn {
		.orangebtn {
			background-image: linear-gradient(to right, #ffb700, #ff8900);
		}
	}

	.sendtime {
		font-size: 13px;
		color: gray;
	}
	.service {
		display: flex;
		width: 90%;
		justify-content: space-between;
	}
}
.desc {
	margin-bottom: 50px;
	position: relative;
	width: 100%;
	top: 500rpx;
	.box {
		width: 100%;
		text-align: center;
		image {
			width: 90%;
		}
	}
	.img {
		width: 100%;
		text-align: center;
		// image {
		// 	width: 100%;
		// 	height: 900rpx;
		// }
	}
	.info {
		width: 100%;
		display: grid;
		grid-template-columns: 50% 50%;
		grid-template-rows: repeat(3, 40px);
		justify-content: center;
		align-items: center;
		border-top: 0.5px solid #e5e5e5;
		box-sizing: border-box;
		.i-1,
		.i-3,
		.i-5 {
			height: 100%;
			width: 100%;
			border-bottom: 0.5px solid #e5e5e5;
			border-right: 0.5px solid #e5e5e5;
			box-sizing: border-box;
			display: flex;
			align-items: center;
			justify-content: flex-start;
			font-size: 14px;
		}
		.i-2,
		.i-4,
		.i-6 {
			font-size: 14px;
			height: 100%;
			width: 100%;
			display: flex;
			align-items: center;
			justify-content: flex-start;
			border-bottom: 0.5px solid #e5e5e5;
			box-sizing: border-box;
		}
		.i-1,
		.i-2,
		.i-5,
		.i-6 {
			background-color: #f7f7f7;
		}
	}
}

.nav {
	position: fixed;
	bottom: 0;
	width: 100%;
	display: grid;
	grid-template-columns: 20% 40% 40%;
	grid-auto-rows: 50px;
	background-color: #ffffff;
	.cartinfo {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		text {
			font-size: 14px;
		}
	}
	.button-box {
		display: flex;
		width: 100%;
		align-items: center;

		.itself {
			width: 240rpx;
			// height: 80rpx;
			border-radius: 20rpx;
			border: none;
			background-image: linear-gradient(to right, #ffb700, #ff8900);
			color: #ffffff;
		}
	}
}
</style>
