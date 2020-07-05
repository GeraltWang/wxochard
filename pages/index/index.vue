<template>
	<view class="container">
		<view>
			<view>
				<view>
					<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" class="swiper" circular="true">
						<swiper-item v-for="(item, index) in banner" :key="index">
							<view>
								<image :src="'http://192.168.123.204:8088/'+item" mode="" class="banner-img"></image>
								<!-- <image :src="'http://192.168.0.167:8088/' + item" mode="" class="banner-img"></image> -->
							</view>
						</swiper-item>
					</swiper>
				</view>
			</view>
			<uni-grid :column="5" :show-border="false" :square="true" class="grid">
				<uni-grid-item class="griditem" v-for="(item, index) in griddata" :key="index">
					<image :src="'http://192.168.123.204:8088/'+item.url" mode="" class="grid-pic"></image>
					<!-- <image :src="'http://192.168.0.167:8088/' + item.url" mode="" class="grid-pic"></image> -->
					<text class="text">{{ item.name }}</text>
				</uni-grid-item>
			</uni-grid>
			<view class="bannerview">
				<view v-for="(item, index) in subtitles" :key="index">
					<view>
						<!-- <navigator :url="'../tab/tab?id='+item.id"><image :src="'http://192.168.0.167:8088/' + item.image" mode="" class="sub-banner-img"></image></navigator> -->
						<navigator :url="'../tab/tab?id='+item.id"><image :src="'http://192.168.123.204:8088/' + item.image" mode="" class="sub-banner-img"></image></navigator>
					</view>
					<view class="jj">
						<scroll-view scroll-x="true" class="scroll">
							<view class="scroll-inside" v-for="tem in item.content" :key="tem.title">
								<navigator :url="'../product/product?id=' + tem.product_id + '&name=' + tem.title + '&url=' + tem.image">
									<image :src="'http://192.168.123.204:8088/'+tem.image" mode="" class="sc-img"></image>
									<!-- <image :src="'http://192.168.0.167:8088/' + tem.image" mode="" class="sc-img"></image> -->
									<view class="limit">
										<text class="text-b">{{ tem.subtitle }}</text>
									</view>
									<view class="limit">
										<text class="text">{{ tem.title }}</text>
									</view>
								</navigator>
								<view class="orangebox">
									<text class="text-color">￥{{ tem.price }}元/{{ tem.volume }}</text>
									<image class="addbtn" src="http://192.168.123.204:8088/images/cart.png" mode="" @click="addCart(tem)"></image>
									<!-- <image class="addbtn" src="http://192.168.0.167:8088/images/cart.png" mode="" @click="addCart(tem)"></image> -->
								</view>
							</view>
						</scroll-view>
					</view>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	created() {
		uni.request({
			url: 'http://192.168.123.204:8088/data/fruits.json',
			// url: 'http://192.168.0.167:8088/data/fruits.json',
			method: 'GET',
			header: {
				'content-type': 'application/json'
			},
			success: res => {
				console.log(res);
				this.banner = res.data.banners;
				this.subtitles = res.data.bannerTags;
			}
		});
	},
	data() {
		return {
			indicatorDots: true,
			autoplay: true,
			interval: 3000,
			duration: 500,
			banner: [],
			subtitles: [],
			cartData: [],
			ts: [],
			griddata: [
				{
					name: '每周上新',
					url: 'images/week.png'
				},
				{
					name: '新客专享',
					url: 'images/new.png'
				},
				{
					name: '天天直播',
					url: 'images/play.png'
				},
				{
					name: '摇一摇',
					url: 'images/shake.png'
				},
				{
					name: '限时特惠',
					url: 'images/limit.png'
				},
				{
					name: '低温速食',
					url: 'images/low.png'
				},
				{
					name: '品质肉禽',
					url: 'images/meat.png'
				},
				{
					name: '新鲜水果',
					url: 'images/fruit.png'
				},
				{
					name: '粮油干货',
					url: 'images/gh.png'
				},
				{
					name: '酒水饮料',
					url: 'images/water.png'
				}
			]
		};
	},
	methods: {
		addCart(item) {
			console.log(item);
			uni.showToast({
				title: '加入购物车成功',
				duration: 1500
			});
			uni.getStorage({
				key: 'cartstorage',
				success(res) {
					console.log(res.data);
					var count = 0;
					var dd = res.data;
					console.log(dd);
					for (var i = 0; i < res.data.length; i++) {
						if (dd[i].id == item.product_id) {
							dd[i].sum++;
							count++;
						}
					}
					if (count == 0) {
						dd.push({
							id: item.product_id,
							name: item.title,
							price: item.price,
							weight: item.volume,
							url: item.image,
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
					console.log(res);
					uni.setStorage({
						key: 'cartstorage',
						data: [
							{
								id: item.product_id,
								name: item.title,
								price: item.price,
								weight: item.volume,
								url: item.image,
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
		}
	}
};
</script>

<style lang="less" scoped>
.swiper {
	width: 100%;
	height: 160px;

	.banner-img {
		width: 100%;
		height: 320rpx;
	}
}

.grid {
	width: 100%;
	display: flex;
	justify-content: center;
	margin-top: 80rpx;
	.grid-item{
		height: 130rpx;
	}
	.grid-pic {
		width: 30px;
		height: 30px;
		place-self: center;
	}
}
.bannerview{
	margin-top: 70rpx;
}
.text {
	font-size: 12px;
	place-self: center;
}

.sub-banner-img {
	width: 100%;
	height: 65px;
}

.jj {
	width: 100%;
	height: 310rpx;
	overflow: hidden;
	// background: #fff;
	white-space: nowrap;

	.scroll {
		height: 290rpx;

		.scroll-inside {
			width: 230rpx;
			height: 265rpx;
			margin-right: 10rpx;
			margin-left: 10rpx;
			display: inline-block;
			overflow: hidden;
			text-align: center;

			.sc-img {
				width: 120rpx;
				height: 120rpx;
			}

			.limit {
				width: 200rpx;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
				margin: auto;
			}

			.text-b {
				font-size: 12px;
				font-weight: bold;
			}

			.text {
				font-size: 12px;
			}

			.orangebox {
				display: flex;
				justify-content: center;
				align-items: center;

				.text-color {
					font-size: 12px;
					place-self: center;
					color: #fe8f00;
				}

				.addbtn {
					width: 37rpx;
					height: 37rpx;
				}
			}
		}
	}
}
</style>
