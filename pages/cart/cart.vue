<template>
	<view>
		<view class="toptext-box">
			<text>商品清单</text>
			<text>总重约{{ weight }}KG</text>
		</view>
		<scroll-view scroll-y="true" class="scroll-list">
			<view>
				<uni-swipe-action>
					<uni-swipe-action-item v-for="(item, index) in data" :key="index" :options="options" @click="del(index)">
						<!-- <van-card :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.123.204:8088/'+item.url" class="cardd"> -->
						<!-- <van-card :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.0.167:8088/' + item.url" class="cardd">
							<view slot="tags">
								<van-tag mark type="primary">次日达</van-tag>
								<van-tag mark type="success">放心购</van-tag>
							</view>
							<view slot="footer">
								<van-checkbox class="pro-check" checked-color="#07c160" :value="item.checked" @change="selected(item)"></van-checkbox>
								<button class="bb" size="mini" type="primary" round @click="reduce(index)">-</button>
								<text>{{ item.sum }}</text>
								<button class="bb" size="mini" type="warn" round @click="add(index)">+</button>
							</view>
						</van-card> -->
						<view class="detail-item">
							<van-checkbox class="check" checked-color="#07c160" :value="item.checked" @change="selected(item)"></van-checkbox>
							<!-- <view class="img"><image :src="'http://192.168.0.167:8088/' + item.url" mode=""></image></view> -->
							<view class="img"><image :src="'http://192.168.123.204:8088/' + item.url" mode=""></image></view>
							<view class="detail-right">
								<text class="t1">{{ item.name }}</text>
								<text class="t3">{{ item.weight }}</text>
								<view>
									<van-tag mark type="primary">次日达</van-tag>
									<van-tag mark type="success">放心购</van-tag>
								</view>
								<view class="detail-right-bottom">
									<view class="bt">
										<text class="bt1">￥{{ item.price }}</text>
									</view>
									<view class="bbbox">
										<van-icon name="arrow-left" size="20px" @click="reduce(index)" />
										<text>{{ item.sum }}</text>
										<van-icon name="arrow" size="20px" @click="add(index)" />
									</view>
								</view>
							</view>
						</view>
					</uni-swipe-action-item>
				</uni-swipe-action>
			</view>
		</scroll-view>
		<view class="sbm">
			<view class="cb"><van-checkbox @change="selectedall" :value="allchecked" class="allcheck">全选</van-checkbox></view>
			<view class="p">
				<text class="sum">总价：{{ getsum }}元</text>
			</view>
			<view class="w">
				<text class="sum">总数：{{ getnum }}件</text>
			</view>
			<view class="b"><button type="warn" @click="checkout">提交订单</button></view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			data: [],
			weight: 0,
			allchecked: false,
			options: [
				{
					text: '删除',
					style: {
						backgroundColor: '#dd524d'
					}
				}
			]
		};
	},
	onShow() {
		let that = this;
		uni.getStorage({
			key: 'cartstorage',
			success(res) {
				that.data = res.data;
			}
		});
		console.log(this.data);
	},
	methods: {
		selected(item) {
			item.checked = !item.checked;
			if (!item.checked) {
				this.allchecked = false;
			} else {
				const test = this.data.every(item => {
					return item.checked === true;
				});
				if (test) {
					this.allchecked = true;
				} else {
					this.allchecked = false;
				}
			}
			uni.setStorage({
				key: 'cartstorage',
				data: this.data,
				success() {
					console.log('success');
				}
			});
			console.log(0);
		},
		selectedall() {
			console.log(1);
			this.allchecked = !this.allchecked;
			if (this.allchecked) {
				this.data.map(item => {
					item.checked = true;
				});
				uni.setStorage({
					key: 'cartstorage',
					data: this.data,
					success() {
						console.log('success');
					}
				});
			} else {
				this.data.map(item => {
					item.checked = false;
				});
				uni.setStorage({
					key: 'cartstorage',
					data: this.data,
					success() {
						console.log('success');
					}
				});
			}
		},
		add(index) {
			console.log(index);
			this.data[index].sum++;
			uni.setStorage({
				key: 'cartstorage',
				data: this.data,
				success() {
					console.log('success');
				}
			});
		},
		reduce(index) {
			console.log(index);
			if (this.data[index].sum > 1) {
				this.data[index].sum--;
			} else {
				this.data[index].sum = 1;
			}
			// this.data[index].sum--;
			uni.setStorage({
				key: 'cartstorage',
				data: this.data,
				success() {
					console.log('success');
				}
			});
		},
		del(index) {
			console.log(index);
			this.data.splice(index, 1);
			uni.setStorage({
				key: 'cartstorage',
				data: this.data,
				success() {
					console.log('success');
				}
			});
		},
		checkout() {
			let flag = this.data.map(item => {
				item.checked = true;
			});
			if (flag.length > 0) {
				uni.showModal({
					title: '提示',
					content: `您购买了${this.getnum}件商品，共计${this.getsum}元
										   确定结算吗？`,
					success: res => {
						if (res.confirm) {
							console.log(this.data);

							var cc = this.data.filter(item => item.checked == false);
							uni.setStorage({
								key: 'cartstorage',
								data: cc,
								success: () => {
									this.data = cc;
								}
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			} else {
				uni.showModal({
					title: '提示',
					content: `购物车空空如也，要去商品页看看吗？`,
					success: res => {
						if (res.confirm) {
							uni.switchTab({
								url: '/pages/index/index'
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			}
		}
	},
	computed: {
		getsum() {
			let totalP = 0;
			this.data.map(item => {
				return item.checked ? (totalP += Number(item.price) * item.sum) : (totalP += 0);
			});
			return totalP.toFixed(2);
		},
		getnum() {
			let totalN = 0;
			this.data.map(item => {
				return item.checked ? (totalN += Number(item.sum)) : (totalN += 0);
			});
			return totalN;
		}
	},
	watch: {
		data: {
			handler(value) {
				var _this = this;
				var count = 0;
				for (var i = 0; i < value.length; i++) {
					if (value[i].checked == true) {
						count++;
					}
				}
				//如果子集全部选中，全选按钮设置选中状态
				if (count == value.length) {
					_this.allchecked = true;
				} else if (value.length == 0) {
					_this.allchecked = false;
				} else {
					_this.allchecked = false;
				}
			},
			deep: true
		}
	}
};
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
	height: 1134rpx;
	position: relative;
	top: 50rpx;
	.cardd {
		width: 100%;
		text {
			font-size: 14px;
		}
	}
	.pro-check {
		position: relative;
		top: 30rpx;
	}
}
.sbm {
	width: 100%;
	height: 100rpx;
	display: grid;
	grid-template-columns: 21% 28% 24% 27%;
	grid-auto-rows: 100rpx;
	align-items: center;
	position: fixed;
	bottom: 0;
	font-size: 13px;
	justify-content: center;
	.cb {
		place-self: center;
	}
	.p {
		place-self: center;
	}
	.w {
		place-self: center;
	}
}
.detail-item {
	width: 100%;
	height: 220rpx;
	display: grid;
	grid-template-columns: 10% 25% 65%;
	grid-auto-rows: 220rpx;
	align-items: center;
	margin-bottom: 10rpx;
	border-bottom: 3rpx solid lightgray;
	box-sizing: border-box;
	.check {
		margin-left: 18rpx;
	}
	.img {
		display: flex;
		width: 100%;
		image {
			width: 170rpx;
			height: 170rpx;
			place-self: center;
			box-sizing: border-box;
			box-shadow: rgba(0, 0, 0, 0.2) 0 1px 5px 0px;
		}
	}
	.detail-right {
		display: grid;
		width: 100%;
		grid-template-columns: 100%;
		grid-auto-rows: 50rpx 70rpx 40rpx 60rpx;
		align-items: center;
		.t1 {
			color: gray;
			font-size: 15px;
		}
		.t2 {
			color: gray;
			font-size: 14px;
		}
		.t3 {
			color: gray;
			font-size: 13px;
		}
		.detail-right-bottom {
			display: flex;
			width: 100%;
			justify-content: space-between;
			.bt {
				width: 300rpx;
			}
			.bt1 {
				display: inline-block;
				width: 125rpx;
				color: #ff8000;
			}
			.bbbox {
				display: flex;
				width: 70%;
				align-items: center;
				justify-content: flex-end;
				margin-right: 20rpx;
			}
		}
	}
}
</style>
