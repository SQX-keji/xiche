<template>
	<view style="padding-bottom: 150upx;">
		<!-- #ifdef MP-WEIXIN -->
		<view style="height: max-content;background-color: #FFFFFF;margin-top: 10upx;padding: 30upx 30upx 10upx 40upx;"
			v-for="(item,index) in list" :key='index' >
			<view @tap='goBackByAddress(item.addressId)'>
				<view style="display: flex;" >
					<view style="font-size: 32rpx;color: #333333;">{{item.name}}</view>
					<view style="font-size: 32rpx;color: #333333;margin-left: 20upx;text-align: right;">{{item.phone}}
					</view>
				</view>
				<view style="display: flex;margin-top: 35rpx;">
					<view style="font-size: 32rpx;color: #333333;width: 90%;">
						{{item.province}}{{item.city}}{{item.district}} {{item.detailsAddress}}
					</view>
				</view>
			</view>
			<view style="margin-top: 15upx;height: 1upx;background-color: #E3E4E5;margin-bottom: 10upx;"></view>
			<view style="display: flex;padding: 10rpx 2rpx 10rpx 0rpx;font-size: 32rpx;">
				<radio-group name="openWay" style="text-align: left;width: 70%;">
					<label class="tui-radio" v-if="item.isDefault == '1'">
						<radio :checked="item.isDefault == 1 ? true : false" active-color="#557EFD" disabled='true'
							style="transform:scale(0.8);" />
						<text style="font-size: 32rpx;margin-left: 10upx;">默认地址</text>
					</label>
				</radio-group>
				<view style="display: flex;margin-left: 80upx;margin-top: 5upx;width: 40%;text-align: right;" v-if="isfa !=1">
					<view style="font-size: 32rpx;color: #999999;width: 50%;" @tap='deleteAddressList(item.addressId)'>
						删除</view>
					<view style="font-size: 32rpx;color: #999999;width: 50%;" @tap='goAddress(item.addressId)'>编辑</view>
				</view>
			</view>
		</view>
		<view style="position: fixed;bottom: 0rpx;left: 0;right: 0;background: #FFFFFF;height:150rpx;">
			<button style="background: #557EFD;color: #FFFFFF;margin: 35rpx;position: fixed;bottom: 0upx;width: 90%;"
				@tap="goAddress('')">
				+新建地址
			</button>
		</view>
		<!-- #endif -->
		<!-- #ifndef MP-WEIXIN -->
		<view style="height: max-content;background-color: #FFFFFF;margin-top: 10upx;padding: 30upx 30upx 10upx 40upx;"
			v-for="(item,index) in list" :key='index' >
			<view @tap='goBackByAddress(item.addressId)'>
				<view style="display: flex;" >
					<view style="font-size: 30upx;color: #333333;">{{item.name}}</view>
					<view style="font-size: 30upx;color: #333333;margin-left: 20upx;text-align: right;">{{item.phone}}
					</view>
				</view>
				<view style="display: flex;margin-top: 30upx;height: 70upx;">
					<view style="font-size: 30upx;color: #333333;width: 90%;">
						{{item.province}}{{item.city}}{{item.district}} {{item.detailsAddress}}
					</view>
				</view>
			</view>
			<view style="margin-top: 30rpx;height: 1upx;background-color: #E3E4E5;margin-bottom: 10upx;"></view>
			<view style="display: flex;padding: 15upx 5upx 15upx 0upx;font-size: 30upx;">
				<radio-group name="openWay" style="text-align: left;width: 70%;">
					<label class="tui-radio" v-if="item.isDefault == '1'">
						<radio :checked="item.isDefault == 1 ? true : false" color="#557EFD" disabled='true'
							style="transform:scale(0.8);" />
						<text style="font-size: 30upx;margin-left: 10upx;">默认地址</text>
					</label>
				</radio-group>
				<view style="display: flex;margin-left: 80upx;margin-top: 5upx;width: 40%;text-align: right;" v-if="isfa !=1">
					<view style="font-size: 30upx;color: #999999;width: 50%;" @tap='deleteAddressList(item.addressId)'>
						删除</view>
					<view style="font-size: 30upx;color: #999999;width: 50%;" @tap='goAddress(item.addressId)'>编辑</view>
				</view>
			</view>
		</view>
		<view style="position: fixed;bottom: 0rpx;left: 0;right: 0;background: #FFFFFF;height:150rpx;">
			<button style="background: #557EFD;color: #FFFFFF;margin: 30upx;position: fixed;bottom: 0upx;width: 90%;"
				@tap="goAddress('')">
				+新建地址
			</button>
		</view>
		<!-- #endif -->
		<!-- 悬浮上拉 -->
		<view class="scroll_top" @tap="topScrollTap" v-bind:class="[scrollTop ? 'active' : '', '']"
			style="bottom: 56px;"><text class="iconfont icon-shangla"></text></view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				openWay: 0,
				list: [],
				// loadingType: 0,
				type: 1,
				scrollTop: false,
				page: 1,
				limit: 5,
				address: '',
				isfa: ''
			}
		},
		onShow() {
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.getAddressList('');
			}
		},
		onLoad(e) {
			this.isfa = e.id
			
			console.log(this.isfa)
			let userId = this.$queue.getData('userId');
			if (userId) {
				this.getAddressList('');
			}
		},
		methods: {
			goBackByAddress(addressId) {
				console.log(addressId)
				this.$queue.setData('EditAddress', addressId);
				console.log(this.isfa)
				if (this.isfa==0) {
					console.log('1111111')
				}if(this.isfa ==1){
					uni.navigateBack()				
				}else if(this.isfa== 3){
					this.updateaddress()
					// uni.navigateBack()
				}
					
			},
			updateaddress() {
				let addressId = this.$queue.getData('EditAddress')
				let data = {
					ordersId: this.isfa.order,
					addressId: addressId
				}
				this.$Request.post('/app/orders/updateOrdersAddress', data).then(res => {
					console.log(res)
					if (res.code == 0) {
						uni.showToast({
							title: '修改成功',
							icon: 'none'
						})
						uni.navigateBack();
						
						
					}
				})

			},
			deleteAddressList(id) {
				console.log(id)
				var id = id
				let data = {
					addressId: id,
				}
				uni.showModal({
					title: '温馨提示',
					content: '您确定要删除此地址信息吗?',
					showCancel: true,
					cancelText: '取消',
					confirmText: '确认',
					success: res => {
						if (res.confirm) {
							this.$Request.postT('/app/address/deleteAddress', data).then(res => {
								console.log(res)
								if (res.code == 0) {
									this.$queue.showToast("删除成功！");
									this.getAddressList();
								} else {
									this.$queue.showToast(res.msg);
								}
							});
						}
					}
				});
			},
			getAddressList(type) {
				// this.loadingType = 1;
				// uni.showLoading({
				// 	title: '加载中...'
				// });
				let userId = this.$queue.getData('userId');
				let data = {
					userId: userId,
					page: this.page,
					limit: this.limit
				}
				this.$Request.getT('/app/address/selectAddressListById', data).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (this.page == 1) this.list = []
						this.list = [...this.list, ...res.data.list]; //追加新数据

						// res.data.list.forEach(d => {
						// 	this.list.push(d);
						// });
						// console.log(this.list)
						// this.loadingType = 0;
					} else {
						// this.loadingType = 2;
					}
					// uni.hideLoading();
					// if (type === 'Refresh') {
					// 	uni.stopPullDownRefresh(); // 停止刷新
					// }
				});
			},
			goAddress(id) {
				uni.navigateTo({
					// url: './EditAddress?id=' + id
					url: '/my/address/Endaddress?id=' + id
				});
			},
			tabSlect(item) {
				this.tabIndex = item.id;
			},
			selectWay(id) {
				this.openWay = id;
			},
			topScrollTap: function() {
				uni.pageScrollTo({
					scrollTop: 0,
					duration: 300
				});
			}
		}
	}
</script>

<style lang="less">
	// @import '../../../static/less/index.less';
	// @import '../../../static/css/index.css';

	page {
		background: #F2F2F2;
	}

	.tui-line-cell {
		width: 100%;
		box-sizing: border-box;
		display: flex;
		align-items: center;
		justify-content: space-between;
		border-bottom: 2upx solid #f2f2f2;
		padding: 0 0 16upx 0;
	}

	.tui-title {
		line-height: 32rpx;
		min-width: 120rpx;
		flex-shrink: 0;
	}

	.tui-input {
		font-size: 32rpx;
		color: #333;
		padding-left: 20rpx;
		flex: 1;
	}
</style>
