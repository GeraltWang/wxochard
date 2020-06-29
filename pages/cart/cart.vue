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
						<van-card :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.123.204:8088/'+item.url" class="cardd">
						<!-- <van-card :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.0.167:8088/' + item.url" class="cardd"> -->
							<view slot="tags">
								<van-tag mark type="primary">次日达</van-tag>
								<van-tag mark type="success">放心购</van-tag>
							</view>
							<view slot="footer">
								<van-checkbox class="pro-check" checked-color="#07c160" :value="item.checked" @change="selected(item)"></van-checkbox>
								<van-button size="mini" type="primary" round @click="reduce(index)">-</van-button>
								<text>{{ item.sum }}</text>
								<van-button size="mini" type="info" round @click="add(index)">+</van-button>
							</view>
						</van-card>
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
			uni.showModal({
				title: '提示',
				content: `您购买了${this.getnum}件商品，共计${this.getsum}元
									  确定结算吗？`,
				success:res=> {
					if (res.confirm) {
						console.log(this.data)
						
						var cc=this.data.filter(item=>item.checked==false)
						uni.setStorage({
							key: 'cartstorage',
							data: cc,
							success:()=> {
								this.data=cc
								
							}
						});
					} else if (res.cancel) {
						console.log('用户点击取消');
					}
				}
			});
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
</style>
