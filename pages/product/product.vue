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
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pname:'',
				page:[],
				indicatorDots: true,
				autoplay: true,
				interval: 3000,
				duration: 500,
			};
		},
		onLoad(option){
			console.log(option)
			this.pname=option.name
			this.request(option)
		},
		methods:{
			request(js){
				uni.request({
					// url: 'http://192.168.123.204:8088/data/fruits.json',
					url: `http://192.168.0.167:8088/data/good_detail_${js.id}.json`,
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: res => {
						// console.log(res);
						this.page=res.data
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
		height: 160px;
	
		.banner-img {
			width: 100%;
			height: 320rpx;
		}
	}

</style>
