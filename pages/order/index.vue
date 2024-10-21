<!-- 菜单悬浮的原理: 通过给菜单添加position:sticky实现, 用法超简单, 仅APP端的低端机不兼容 https://caniuse.com/#feat=css-sticky -->
<template>
	<view>
		<view class="sticky-tabs bg head">
			<me-tabs v-model="tabIndex" nameKey='title' :tabs="tabs" @change="tabChange"></me-tabs>
		</view>
		<view v-if="goods.length > 0" class="margin-lr-sm margin-top-16 padding-sm bg radius"
			v-for="(item,index) in goods" :key='index' @click="goNav('/my/order/pay?id='+item.ordersId+'&isTrue=1')">
			<view class="flex justify-between text-26">
				<!-- 	<view class="text-blue" v-if="item.state ==0">待付款</view>
					<view class="text-blue" v-if="item.state ==1">进行中</view>
					<view class="text-blue" v-if="item.state ==2">已完成</view>
					<view class="text-blue" v-if="item.state ==3">已取消</view> -->
				<view class="text-blue">{{item.statusName}}</view>
				<view style="color: #999999;">{{item.updateTime}}</view>
			</view>
			<view class="flex u-p-t-30">
				<view class="u-m-r-10">
					<u-avatar :src="item.homepageImg?item.homepageImg: '../../static/logo.png'" mode="square"
						size="100">
					</u-avatar>
				</view>
				<view class="u-flex-1 text-white margin-left-xs">
					<view class="text-30  text-bold u-line-1" style="width: 560rpx;">{{item.myLevel}}</view>
					<view class="flex-wrap flex margin-top-xs">
						<view class="flex align-center margin-right-xs text-sm" style="color: #999999;"
							v-for="(ite,ind) in item.gameName" :key="ind">
							{{ite}}
							<view style="width: 1rpx;height: 21upx;background: #CCCCCC;margin-left: 10upx;"
								v-if="item.gameName.length-1!=ind"></view>
						</view>
					</view>
					<!-- <view class="u-font-14 margin-top-xs u-tips-color flex justify-between">
							服务地址：{{item.city}}{{item.district}}{{item.detailsAddress}}
						</view> -->
				</view>
			</view>
			<view class="flex u-p-t-20 justify-between align-center">
				<view class="text-white flex-sub ">
					实付：<text class="text-df">￥</text><text class="text-xl text-bold">{{item.payMoney}}</text>
				</view>
				<view class="flex text-right">
					<view class="btn" v-if="item.state == 1" @click.stop="look(item)">查看确认码</view>
					<view v-if="item.state == 0||item.state == 4" @click.stop="cancelOrder(item)" class="btn">取消订单
					</view>
					<view v-if="item.state == 0" @click.stop="goNav('/my/order/pay?id='+item.ordersId+'&isTrue=0')"
						class="btn">去支付</view>
					<!-- <view v-if="item.state == 1" @click="goNav('/my/order/pay?id='+item.ordersId+'&isTrue=1')" class="btn">查看详情</view> -->

					<view v-if="item.state == 1" @click.stop="cancel(item)" class="btn">订单完成</view>
					<view v-if="item.state == 6" @click.stop="cancel(item)" class="btn">确认完成</view>
					<view v-if="item.state == 2"
						@click.stop="getOrder(item.ordersId)"
						class="btn">去投诉</view>
					<view v-if="item.state == 2 && item.commentCount == 0"
						@click.stop="goNav('/my/order/feedback?id='+item.orderTakingId+ '&ordersId='+item.ordersId)"
						class="btn">去评价</view>
					<!-- <view v-if="item.state == 3" @click.stop="delOrder(item)" class="btn">删除订单</view> -->

				</view>
			</view>
		</view>
		<empty v-if="goods.length == 0" content="暂无数据"></empty>

		<view class="boxtk" v-if="reverTrue">
			<view class="whitebox padding">
				<view class="flex justify-between align-center">
					<view style="font-size:38upx;color:#333333" class="text-bold">确认码</view>
					<view @click="bindclose()">
						<image src="/static/images/msg/close.png" style="width:35upx;height:35upx;"></image>
					</view>
				</view>

				<view class="margin-top-xl text-center" style="font-size: 88upx;">
					{{code}}
				</view>

			</view>
		</view>

	</view>
</template>

<script>
	import meTabs from "@/components/mescroll-uni/me-tabs/me-tabs.vue";
	import empty from '@/components/empty.vue'

	export default {
		components: {
			meTabs,
			empty
		},
		data() {
			return {
				goods: [], // 数据列表
				game: [],
				tabs: [{
					title: '全部',
					status: ''
				}, {
					title: '待付款',
					status: '0'
				}, {
					title: '待接单',
					status: '4'
				}, {
					title: '待服务',
					status: '5'
				},{
					title: '待确认',
					status: '6'
				}, {
					title: '进行中',
					status: '1'
				}, {
					title: '已完成',
					status: '2'
				}, {
					title: '已取消',
					status: '3'
				}],
				tabIndex: 0, // tab下标

				page: 1,
				limit: 10,
				userId: 0,
				status: 1,
				nickName: '',
				avatar: '',
				reverTrue: false,
				code: '',
				count: ''
			}
		},
		onLoad() {
			// this.$queue.showLoading("加载中...");
			
			this.nickName = uni.getStorageSync('nickName')
			// this.getlist()
		},
		onShow() {
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.getlist()
			}

		},
		methods: {
			getOrder(id) {
				let data = {
					id: id
				}
				this.$Request.get('/app/orders/queryOrders', data).then(res => {
					if (res.code == 0) {
						let item = res.data
						uni.navigateTo({
							url:'/my/order/complain?id='+item.ordersId+'&byUserId='+item.orderTakingUserId+'&userName='+item.user.userName+'&title='+item.orderTaking.myLevel+'&ordersNo='+item.ordersNo
						})
						// this.order = res.data
						// if (this.order.orderTaking.gameId) {
						// 	this.order.orderTaking.gameId = this.order.orderTaking.gameId.split(",");
						// }
						// if (this.order.startImg) {
						// 	this.order.startImg = this.order.startImg.split(",");
						// }
						// if (this.order.endImg) {
						// 	this.order.endImg = this.order.endImg.split(",");
						// }
						// if (this.order.carPhone) {
						// 	let carPhone = this.order.carPhone
						// 	this.order.carPhone = carPhone.substring(0, 3) + "****" + carPhone.substr(carPhone
						// 		.length - 4);
						// }
						// console.log(this.order.carPhone, 'vipvipvipv')
					}
				})
			},
			bindclose() {
				this.reverTrue = false
			},
			look(e) {
				this.reverTrue = true
				this.code = e.code
			},
			getlist() {
				let curTab = this.tabs[this.tabIndex].status

				let data = {
					status: curTab,
					page: this.page,
					limit: this.limit
				}
				this.$Request.get('/app/orders/selectMyOrder', data).then(res => {
					if (res.code == 0) {
						this.count = res.data.totalCount
						if (this.page == 1) {
							this.goods = []
						}
						res.data.list.forEach(d => {
							if (d.gameName) {
								d.gameName = d.gameName.split(',')
							}
							if (d.state == 0) {
								d.statusName = '待付款'
							} else if (d.state == 1) {
								d.statusName = '进行中'
							} else if (d.state == 2) {
								d.statusName = '已完成'
							} else if (d.state == 3) {
								d.statusName = '已取消'
							} else if (d.state == 4) {
								d.statusName = '待接单'
							} else if (d.state == 5) {
								d.statusName = '待服务'
							} else if (d.state == 6) {
								d.statusName = '待确认'
							}
							this.goods.push(d);
						});
					}
				}).catch(() => {
					//联网失败, 结束加载
					uni.showToast({
						title: res.msg,
						icon: 'none'
					})
				});
			},
			// 切换菜单
			tabChange() {
				this.goods = []; // 置空列表,显示加载进度条
				this.page = 1
				this.getlist()
			},
			// 取消订单
			cancelOrder(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '确认取消订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
								status: '3'
							}
							that.$Request.get('/app/orders/cancelOrder', data).then(res => {
								if (res.code == 0) {
									that.page = 1
									that.getlist()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 完成订单
			cancels(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '订单完成后款项将支付给服务方，确认完成订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
								status: '6'
							}
							that.$Request.get('/app/orders/cancelOrder', data).then(res => {
								if (res.code == 0) {
									that.page = 1
									that.getlist()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 完成订单
			cancel(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '订单完成后款项将支付给服务方，确认完成订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
								status: '2'
							}
							that.$Request.get('/app/orders/cancelOrder', data).then(res => {
								if (res.code == 0) {
									that.page = 1
									that.getlist()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			//删除
			delOrder(e) {
				let that = this
				uni.showModal({
					title: '提示',
					content: '确定删除订单吗?',
					success: function(res) {
						if (res.confirm) {
							let data = {
								id: e.ordersId,
							}
							that.$Request.get('/app/orders/deleteOrder', data).then(res => {
								if (res.code == 0) {
									uni.showToast({
										title: "删除成功"
									})
									that.page = 1
									that.getlist()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			goNav(url) {
				uni.navigateTo({
					url
				})
			}
		},
		onReachBottom: function() {
			if (this.count == this.goods.length) {
				uni.showToast({
					title: '已经到底了',
					icon: 'none'
				})
			} else {
				this.page = this.page + 1;
				this.getlist()
			}
		},
		onPullDownRefresh: function() {
			this.page = 1;
			this.getlist()
		},
	}
</script>

<style lang="scss">
	/*
	sticky生效条件：
	1、父元素不能overflow:hidden或者overflow:auto属性。(mescroll-body设置:sticky="true"即可, mescroll-uni本身没有设置overflow)
	2、必须指定top、bottom、left、right4个值之一，否则只会处于相对定位
	3、父元素的高度不能低于sticky元素的高度
	4、sticky元素仅在其父元素内生效,所以父元素必须是 mescroll
	*/
	.sticky-tabs {
		z-index: 990;
		position: sticky;
		top: var(--window-top);
		// background-color: #fff;
	}

	// 使用mescroll-uni,则top为0
	.mescroll-uni,
	/deep/.mescroll-uni {
		.sticky-tabs {
			top: 0;
		}
	}

	.head {
		/* #ifdef APP */
		padding-top: 100upx;
		/* #endif */
		
	}

	.demo-tip {
		padding: 18upx;
		font-size: 24upx;
		text-align: center;
	}

	page {
		background-color: #F7F7F7;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.btn {
		color: #0175FE;
		background: #D9EBFF;
		padding: 13upx 30upx;
		border-radius: 50upx;
		margin-left: 20upx;
		font-size: 24upx;
	}

	.boxtk {
		width: 100%;
		height: 162vh;
		background: rgba(0, 0, 0, 0.7);
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		z-index: 99;

	}

	.whiteboxs {
		// width: 461upx;
		// height: 563upx;
		text-align: -webkit-center;
		border-radius: 32upx;
		margin: 0 auto;
		position: fixed;
		top: 185px;
		left: 0;
		right: 0;
		z-index: 99;
	}

	.whitebox {
		width: 550upx;
		height: 350upx;
		background: #FFFFFF;
		border-radius: 32upx;
		margin: 0 auto;
		position: fixed;
		top: 450upx;
		left: 0;
		right: 0;
		z-index: 99;
	}
</style>
