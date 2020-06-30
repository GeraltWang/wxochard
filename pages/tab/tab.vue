<template>
	<view>
		<van-tabs swipeable >
		  <van-tab v-for="item in data" :title="item.name" :key="item.id">
			  <view class="detail-item" v-for="(tem,index) in item.productGroup" :key="index">
			  	<view class="img">
			  		<image :src="'http://192.168.0.167:8088/' + tem.photo" mode=""></image>
			  	</view>
				<view class="detail-right">
					<text class="t1">{{tem.product_name}}</text>
					<text class="t2">{{tem.product_desc}}</text>
					<text class="t3">{{tem.volume}}</text>
					<view class="detail-right-bottom">
						<view class="bt">
							<text class="bt1">￥{{tem.price}}</text>
						<text class="bt2" v-for="(em,index) in tem.cart_tag_list" :key="index">{{em}}</text>
						</view>
						<image class="addbtn" src="http://192.168.0.167:8088/images/cart.png" mode="" @click="addCart(tem)"></image>
					</view>
				</view>
			  </view>
		  </van-tab>
		</van-tabs>
		<view class="float" @click="jump">
			<van-icon name="cart" :info="cartnum" color="white"/>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				data:[]
			};
		},
		onLoad(option) {
			console.log(option);
			this.request(option)
			
		},
		methods:{
			jump(){
				uni.switchTab({
					url: '/pages/cart/cart'
				});
			},
			request(js) {
				uni.request({
					// url: `http://192.168.123.204:8088/data/good_detail_${js.id}.json`,
					url: `http://192.168.0.167:8088/data/sub_category_list_${js.id}.json`,
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: res => {
						// console.log(res);
						this.data=res.data
						console.log(this.data)
					}
				});
			},
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
							if (dd[i].id == item.id) {
								dd[i].sum++;
								count++;
							}
						}
						if (count == 0) {
							dd.push({
								id: item.id,
								name: item.product_name,
								price: item.price,
								weight: item.volume,
								url: item.photo,
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
									id: item.id,
									name: item.product_name,
									price: item.price,
									weight: item.volume,
									url: item.photo,
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
	}
</script>

<style lang="less" scoped>
	.float{
		width: 80rpx;
		height: 80rpx;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		position: fixed;
		background-color: #4D8BFB;
		bottom: 100rpx;
		left: 20rpx;
	}
	.detail-item{
		width: 100%;
		height: 300rpx;
		display: grid;
		grid-template-columns: 25% 75%;
		grid-auto-rows: 220rpx;
		justify-items: center;
		.img{
			display: flex;
			width: 100%;
			image{
				width: 170rpx;
				height: 170rpx;
				place-self: center;
			}
		}
		.detail-right{
			display: grid;
			width: 100%;
			grid-template-columns: 100%;
			grid-auto-rows: 50rpx 70rpx 40rpx 60rpx;
			.t1{
				color: gray;
				font-size: 15px;
			}
			.t2{
				color: gray;
				font-size: 14px;
			}
			.t3{
				color: gray;
				font-size: 13px;
			}
			.detail-right-bottom{
				display: flex;
				width: 100%;
				justify-content: space-between;
				.bt{
					width: 300rpx;
				}
				.bt1{
					display: inline-block;
					width: 125rpx;
					color: #FF8000;
				}
				.bt2{
					font-size: 14px;
					background-color: #FF8000;
					color: #FFFFFF;
					border-radius: 10rpx;
				}
				.addbtn {
					margin-right: 10rpx;
					width: 45rpx;
					height: 45rpx;
				}
			}
		}
	}
</style>
