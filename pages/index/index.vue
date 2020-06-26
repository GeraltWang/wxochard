<template>
	<view class="container">
		<view>
			<view>
				<view>
					<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" class="swiper"
					 circular="true">
						<swiper-item v-for="(item,index) in banner" :key=index>
							<view>
								<image :src="'http://192.168.123.204:8088/'+item" mode="" class="banner-img"></image>
							</view>
						</swiper-item>
					</swiper>
				</view>
			</view>
			<uni-grid :column="5" :show-border="false" :square="false" class="grid">
				<uni-grid-item class="griditem" v-for="(item,index) in griddata" :key=index>
					<image :src="'http://192.168.123.204:8088/'+item.url" mode="" class="grid-pic"></image>
					<text class="text">{{item.name}}</text>
				</uni-grid-item>
			</uni-grid>
			<view>
				<view v-for="(item,index) in subtitles" :key=index>
					<view>
						<image :src="'http://192.168.123.204:8088/'+item.image" mode="" class="sub-banner-img"></image>
					</view>
					<view class="jj">
						<scroll-view scroll-x="true" class="scroll">
							<view class="scroll-inside" v-for="(tem,index) in item.content" :key=index>
								<image :src="'http://192.168.123.204:8088/'+tem.image" mode="" class="sc-img"></image>
								<view class="limit">
									<text class="text-b">{{tem.subtitle}}</text>
								</view>
								<view class="limit">
									<text class="text">{{tem.title}}</text>
								</view>
								<view class="orangebox">
									<text class="text-color">￥{{tem.price}}元/{{tem.volume}}</text>
									<image class="addbtn" src="http://192.168.123.204:8088/images/cart.png" mode="" @click="addCart(tem)"></image>
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
				method: 'GET',
				header: {
					'content-type': 'application/json'
				},
				success: (res) => {
					console.log(res);
					this.banner = res.data.banners
					this.subtitles = res.data.bannerTags
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
				griddata: [{
						name: '每周上新',
						url: 'images/week.png'
					}, {
						name: '新客专享',
						url: 'images/new.png'
					}, {
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
			}
		},
		methods: {
			addCart(item) {
				this.cartData.push(item)
				let currentCart = uni.getStorage({
					key: 'cartstorage',
					success: function(res) {
						console.log(res.data);
					}
				});
				if(!currentCart){
					uni.setStorage({
					    key: 'cartstorage',
					    data: this.cartData,
					    success(res) {
					        console.log(res);
					    }
					});
				}else{
					let cdata=JSON.parse(currentCart)
					cdata.push(item)
					uni.setStorage({
					    key: 'cartstorage',
					    data: cdata,
					    success(res) {
					        console.log(res);
					    }
					});
				}
			}
		}
	}
</script>

<style lang="less" scoped>
	.swiper {
		width: 100%;
		height: 240px;

		.banner-img {
			width: 100%;

		}
	}

	.grid {
		width: 100%;
		display: flex;
		justify-content: center;
		margin-top: 20rpx;

		.grid-pic {
			width: 30px;
			height: 30px;
			place-self: center;
		}
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
						color: #FE8F00;
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
