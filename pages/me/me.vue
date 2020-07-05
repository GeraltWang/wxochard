<template>
	<view class="all">
		<view class="user">
			<view class="userinfo" v-if="login">
				
					<view class="top">
						<image class="userinfo-img" :src="user.avatarUrl"></image>
						<view class="top-text">
							<text>{{ user.nickName }}</text>
							<van-button color="linear-gradient(to right, #4bb0ff, #6149f6)" size="small" round>签到送好礼</van-button>
						</view>
					</view>
					<view class="grid">
						<view class="grid-item">
							<text>积分</text>
							<text>-</text>
						</view>
						<view class="grid-item">
							<text>资产·充值</text>
							<text>-</text>
						</view>
						<view class="grid-item">
							<text>优惠券</text>
							<text>-</text>
						</view>
						<view class="grid-item">
							<text>礼品</text>
							<text>-</text>
						</view>
					</view>
				
			</view>
			<view class="userinfo" v-else>
				<view class="top">
					<!-- <image class="userinfo-img" src="http://192.168.0.167:8088/images/xiaotian.png"></image> -->
					<image class="userinfo-img" src="http://192.168.123.204:8088/images/xiaotian.png"></image>
					<view class="top-text">
						<text @click="loginclick">登录/注册</text>
						<van-button color="linear-gradient(to right, #4bb0ff, #6149f6)" size="small" round>签到送好礼</van-button>
					</view>
				</view>
				<view class="grid">
					<view class="grid-item">
						<text>积分</text>
						<text>-</text>
					</view>
					<view class="grid-item">
						<text>资产·充值</text>
						<text>-</text>
					</view>
					<view class="grid-item">
						<text>优惠券</text>
						<text>-</text>
					</view>
					<view class="grid-item">
						<text>礼品</text>
						<text>-</text>
					</view>
				</view>
			</view>
		</view>
		<view class="mylist">
			<view class="list-top">
				<text>我的订单</text>
				<text>全部订单></text>
			</view>

			<uni-grid :column="5" :show-border="false" :square="false" class="gridd">
				<uni-grid-item v-for="(item, index) in listgrid" :key="index" class="gridd-item">
					<van-icon :name="item.logo" size="30px"/>
					<text class="grid-text">{{ item.name }}</text>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="mylist">
			<view class="list-top">
				<text>我的订单</text>
				<text>全部订单></text>
			</view>
		
			<uni-grid :column="5" :show-border="false" :square="false" class="gridd">
				<uni-grid-item v-for="(item, index) in listgrid" :key="index" class="gridd-item">
					<van-icon :name="item.logo" size="30px"/>
					<text class="grid-text">{{ item.name }}</text>
				</uni-grid-item>
			</uni-grid>
		</view>
		<view class="foot">
			<view class="gridd">
				<view class="gridd-item" v-for="(item, index) in lastgrid" :key="index">
					<van-icon :name="item.logo" size="30px"/>
					<text class="grid-text">{{ item.name }}</text>
				</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		return {
			user: [],
			login: false,
			loading: true,
			listgrid: [
				{ name: '待付款', logo: 'peer-pay' },
				{ name: '待发货', logo: 'send-gift-o' },
				{ name: '待收货', logo: 'logistics' },
				{ name: '待评价', logo: 'comment-o' },
				{ name: '退换货', logo: 'exchange' }
			],
			lastgrid: [
				{ name: '会员中心', logo: 'gem-o' },
				{ name: '直播间', logo: 'tv-o' },
				{ name: '天天新鲜屋', logo: 'new-arrival-o' },
				{ name: '卡券中心', logo: 'discount' },
				{ name: '企业购', logo: 'friends-o' },
				{ name: '在线客服', logo: 'service-o' },
				{ name: '我的问答', logo: 'exchange' },
			]
		};
	},
	methods: {
		loginclick() {
			let that = this;
			uni.login({
				provider: 'weixin',
				success: function(loginRes) {
					// 获取用户信息
					uni.getUserInfo({
						provider: 'weixin',
						success: function(infoRes) {
							that.user = infoRes.userInfo;
							that.login = true;
							console.log(that.user);
						}
					});
				}
			});
		}
	}
};
</script>

<style lang="less" scoped>
.all {
	// display: flex;
	// flex-direction: column;
	// width: 100%;
	// align-items: center;
	background-color: #FDFDFD;
}
.user {
	width: 100%;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	margin-top: 10rpx;
	
}

.userinfo {
	display: flex;
	width: 90%;
	height: 150px;
	justify-content: center;
	flex-direction: column;
	border-radius: 15rpx;
	box-sizing: border-box;
	box-shadow: rgba(0, 0, 0, 0.2) 0 1px 5px 0px;
	background-color: #F2F1ED;
	.top {
		display: flex;
		width: 100%;
		justify-content: flex-start;
		margin-left: 40rpx;
		flex-direction: row;
		.userinfo-img {
			width: 120upx;
			height: 120upx;
			border-radius: 50%;
		}
		.top-text {
			display: flex;
			flex-direction: column;
			// align-items: center;
			margin-left: 20upx;
		}
	}
	.grid {
		display: flex;
		width: 100%;
		height: 100rpx;
		justify-content: space-around;
		align-items: center;
		.grid-item {
			display: flex;
			flex-direction: column;
			align-items: center;
			font-size: 14px;
			text{
				font-size: 14px;
			}
		}
	}
}
.mylist {
	display: flex;
	width: 90%;
	height: 150px;
	flex-direction: column;
	justify-content: space-around;
	margin: auto;
	border-radius: 15rpx;
	margin-top: 20rpx;
	box-sizing: border-box;
	box-shadow: rgba(0, 0, 0, 0.2) 0 1px 5px 0px;
	.list-top{
		display: flex;
		width: 100%;
		justify-content: space-between;
		margin-top: 10px;
		margin-bottom: 40px;
		text:nth-child(1)
		{
		margin-left: 26rpx;
		font-size: 14px;
		}
		text:nth-child(2)
		{
		margin-right: 26rpx;
		font-size: 14px;
		}
	}
}
.gridd {
	width: 100%;
	height: 100rpx;
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 20rpx;
	margin: auto;
	.gridd-item {
		display: flex;
		place-self: center;
		
	}
	.grid-text{
		place-self: center;
		font-size: 14px;
	}
}
.foot{
	width: 100%;
	margin-top: 30rpx;
	display: flex;
	.gridd {
		width: 100%;
		grid-template-columns: repeat(4,25%);
		grid-template-rows: repeat(2,150rpx);
		display: grid;
		justify-content: center;
		flex-direction: column;
		align-items: center;
		.gridd-item {
			width: 100%;
			display: flex;
			flex-direction: column;
			place-self: center;
			
		}
		.grid-text{
			place-self: center;
			font-size: 14px;
		}
	}
}
</style>
