<template>
	<view>
		<swiper :indicator-dots="indicatorDots" :autoplay="autoplay" :interval="interval" :duration="duration" class="swiper"
		 circular="true">
			<swiper-item v-for="(item, index) in page.templatePhoto" :key="index">
				<view>
					<image :src="'http://192.168.123.204:8088/'+item" mode="" class="banner-img"></image>
					<!-- <image :src="'http://192.168.0.167:8088/' + item" mode="" class="banner-img"></image> -->
				</view>
			</swiper-item>
		</swiper>
		<view class="card">
			<view class="title">
				{{pname}}
			</view>
			<view class="subtitle">
				{{page.productInfo.product_desc}}
			</view>
			<view class="price">
				￥{{page.productInfo.price}}
			</view>
			<view class="btn">
				<van-button color="linear-gradient(to right, #FFB700, #FF8900)" size="small">{{page.productItem[0].volume}}</van-button>
			</view>
			<view class="sendtime">
				{{page.sendTimeMsg}}
			</view>
			<view class="service">
				<text>
					<van-icon name="arrow" color="red"/>
					<text>保障</text>
				</text>
				<text>{{page.refundRule}}</text>
				<van-icon name="arrow" color="black"/>
			</view>
		</view>
		<view class="img">
			<image :src="'http://192.168.0.167:8088/' + item" mode="" v-for="(item,index) in page.templateInfo.desc_imgs" :key='index'></image>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pname: '',
				page: [],
				indicatorDots: true,
				autoplay: true,
				interval: 3000,
				duration: 500,
			};
		},
		onLoad(option) {
			console.log(option)
			this.pname = option.name
			this.request(option)
		},
		methods: {
			request(js) {
				uni.request({
					url: `http://192.168.123.204:8088/data/good_detail_${js.id}.json`,
					// url: `http://192.168.0.167:8088/data/good_detail_${js.id}.json`,
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: res => {
						// console.log(res);
						this.page = res.data
						console.log(this.page);
					}
				});
			}
		}
	}
</script>

<style lang="less" scoped>
	.swiper {
		width: 100%;
		height: 290px;
		// text-align: center;

		.banner-img {
			width: 100%;
			height: 600rpx;
			// border-radius: 15rpx;
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
		background-color: #FFFFFF;

		.title {
			font-weight: bold;
		}

		.subtitle {
			color: gray;
		}

		.price {
			color: #FF8000;
			font-size: 22px;
			font-weight: bold;
		}

		.btn {}

		.sendtime {
			font-size: 13px;
			color: gray;
		}
		.service{
			display: flex;
			width: 90%;
			justify-content: space-between;
			
		}
	}
	.img{
		width: 100%;
	}
</style>
