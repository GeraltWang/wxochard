<template>
	<view>
		<view class="toptext-box">
			<text>商品清单</text>
			<text>总重约{{ weight }}KG</text>
		</view>
		<scroll-view scroll-y="true" class="scroll-list">
			<view>
				<!-- <van-card :price="item.price" :desc=item.volume :title=item.title :thumb="'http://192.168.123.204:8088/'+item.image"> -->
				<uni-swipe-action>
					<uni-swipe-action-item v-for="(item, index) in data" :key="index" :options="options" @click="onClick" @change="change">
						<van-card :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.0.167:8088/' + item.url" class="cardd">
							<view slot="footer">
								<van-checkbox class="pro-check" checked-color="#07c160" :value="item.checked" @change="selected(item)"></van-checkbox>
								<van-button size="mini" type="primary">-</van-button>
								<text>{{ item.sum }}</text>
								<van-button size="mini" type="info">+</van-button>
							</view>
						</van-card>
					</uni-swipe-action-item>
				</uni-swipe-action>
				<!-- <van-card  :price="item.price" :desc="item.weight" :title="item.name" :thumb="'http://192.168.0.167:8088/' + item.url">
					<view slot="footer">
						<van-checkbox class="pro-check" checked-color="#07c160"></van-checkbox>
						<van-button size="mini" type="primary">-</van-button>
						<text>{{item.sum}}</text>
						<van-button size="mini" type="info">+</van-button>
					</view>
				</van-card> -->
			</view>
		</scroll-view>
		<van-submit-bar :price="3050" button-text="提交订单" @submit="onSubmit"><van-checkbox @change="selectedall" :value="allchecked">全选</van-checkbox></van-submit-bar>
	</view>
</template>

<script>
export default {
	data() {
		return {
			data: [],
			weight: 0,
			total: 0,
			allchecked: true,
			options: [
				{
					text: '取消',
					style: {
						backgroundColor: '#007aff'
					}
				},
				{
					text: '确认',
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
	height: 1150rpx;
	// margin-bottom: 130rpx;
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
</style>
