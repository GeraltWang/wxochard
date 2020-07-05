<template>
	<view class="main">
		<view class="side">
			<van-sidebar :active-key="activeKey">
				<van-sidebar-item :title="item.name" :key="item.id" v-for="item in categoryList" @click="switchTab(item.id)" />
			</van-sidebar>
		</view>
		<view class="list">
			<view class="" v-for="(item,index) in subCategoryList" :key='index'>
				<view class="card">
					<view class="card-text">
						<text>{{ item.className.name }}</text>
					</view>
					<view class="card-item" v-for="(tem, index) in item.classGroup" :key="index">
						<image :src="'http://192.168.123.204:8088/'+tem.class_photo" mode="" class="card-item-img"></image>
						<!-- <image :src="'http://192.168.0.167:8088/' + tem.class_photo" mode="" class="card-item-img"></image> -->
						<text class="card-item-text">{{ tem.name }}</text>
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
				url: 'http://192.168.123.204:8088/data/category_list.json',
				// url: 'http://192.168.0.167:8088/data/category_list.json',
				method: 'GET',
				header: {
					'content-type': 'application/json'
				},
				success: res => {
					this.categoryList = res.data;
					this.header = res.data.className;
					console.log(this.categoryList);
				}
			});
			uni.request({
				url: `http://192.168.123.204:8088/data/category_list_${this.id}.json`,
				// url: `http://192.168.0.167:8088/data/category_list_${this.id}.json`,
				method: 'GET',
				header: {
					'content-type': 'application/json'
				},
				success: res => {
					this.subCategoryList = res.data;
					console.log(this.subCategoryList);
				}
			});
		},
		data() {
			return {
				categoryList: [],
				subCategoryList: [],
				header: '',
				scrollTop: 0,
				scrollHeight: 0,
				activeKey: 0,
				id: 478
			};
		},
		methods: {
			switchTab(e) {
				console.log(e);
				this.id = e;
				uni.request({
					url: `http://192.168.123.204:8088/data/category_list_${this.id}.json`,
					// url: `http://192.168.0.167:8088/data/category_list_${this.id}.json`,
					method: 'GET',
					header: {
						'content-type': 'application/json'
					},
					success: res => {
						this.subCategoryList = res.data;
						console.log(this.subCategoryList);
					}
				});
			}
		}
	};
</script>

<style scoped lang="less">
	.main {
		display: flex;
		flex-direction: row;
		width: 100%;

		.side {
			width: 191rpx;
		}

		.list {
			display: flex;
			flex-direction: column;
			width: 72%;
			
			border-radius: 15rpx;
			margin-bottom: 20rpx;
			box-shadow: rgba(0, 0, 0, 0.2) 0 1px 5px 0px;
		}

		.card-text {
			margin-top: 30rpx;
			display: flex;
			justify-content: center;
			width: 100%;
			font-size: 18px;
			font-weight: bold;
		}

		.card {
			display: flex;
			flex-direction: row;
			flex-wrap: wrap;
			align-items: center;
		}

		.card-item {
			display: flex;
			width: 180rpx;
			height: 180rpx;
			flex-direction: column;
			justify-content: center;
			align-items: center;

			.card-item-img {
				width: 100rpx;
				height: 100rpx;
			}

			.card-item-text {
				font-size: 14px;
			}
		}
	}
</style>
