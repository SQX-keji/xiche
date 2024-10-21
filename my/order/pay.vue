<template>
	<view>
		<view class="padding-lr padding-bottom">
			<view class="bg padding radius margin-top">
				<view class="flex " @click="binddetail(order)">
					<view class="u-m-r-10" v-if="order.orderTaking">
						<!-- <u-avatar :src="order.user.avatar" size="68"></u-avatar> -->
						<image :src="order.orderTaking.homepageImg" style="width: 140rpx;height: 140rpx;border-radius: 10rpx;"></image>
					</view>
					<view class="u-flex-1 text-white">
						<view class="u-font-14  text-bold padding-bottom-xs u-line-1" style="width: 480rpx;" v-if="order.orderTaking">
							{{order.orderTaking.myLevel}}
						</view>
						<view class="flex-wrap flex margin-top-xs" v-if="order.orderTaking">
							<view class="flex align-center margin-right-xs text-sm" style="color: #999999;"
								v-for="(ite,ind) in order.orderTaking.gameId" :key="ind">
								{{ite}}
								<view style="width: 1rpx;height: 21upx;background: #CCCCCC;margin-left: 10upx;"
									v-if="order.orderTaking.gameId.length-1!=ind"></view>
							</view>
						</view>
						<view class="padding-top-sm text-lg" style="color: #FF2D01;">
							<text style="font-size: 24rpx;">￥</text>
							<!-- {{isVip? order.orderTaking.memberMoney :order.orderTaking.money}} -->
							{{order.payMoney}}
						</view>
					</view>
				</view>
			</view>

			<view class="bg padding radius margin-top">
				<view v-if="order.state==1||order.state==2||order.state==5">
					<view class="">联系洗车工 </view>
					<view class="margin-tb-xs flex align-center"
						style="font-size: 30upx;font-weight: bold;color: #000;">
						<view>{{order.user.userName}}</view>
						<view @click="bindphone(order.user.phone)" class="margin-left-sm">
							<u-icon name="phone"></u-icon>
						</view>
					</view>
				</view>
				<view class="flex align-center " @tap="bindGps(order.latitude,order.longitude,order.city)">
					<view class="tit">
						{{order.city}}
						<u-icon name="map"></u-icon>
					</view>
				</view>
			</view>

			<!-- <view class="bg padding radius margin-top">
				<view class="flex">
					<view class="">
						<view class="" v-if="order.state==1||order.state==2||order.state==5">联系洗车工 </view>
						<view class="margin-tb-xs" v-if="order.state==1||order.state==2||order.state==5">{{order.user.userName}} {{order.user.phone}}</view>
						<view class="flex align-center ">
							<view>
								<image src="../../static/images/my/dizhi.png" style="width: 32upx;height: 32upx;">
								</image>
							</view>
							<view class="text-sm margin-left-xs">
								<view
									style="overflow: hidden;text-overflow: ellipsis;word-wrap: break-word;white-space: normal;-webkit-line-clamp: 2;">
									{{order.city}}
								</view>
							</view>
						</view>
					</view>
					<view class="margin-lr" style="width: 1rpx;height: auto;background: #F2F2F2;"></view>
					<view class="flex align-center">
						<view class="" @tap="bindGps(order.latitude,order.longitude,order.city)">
							<image src="../../static/images/my/address.png" style="width: 38upx;height: 49upx;"></image>
						</view>
						<view class="margin-left-xl" @click="bindphone(order.user.phone)"
							v-if="order.state==1||order.state==2||order.state==5">
							<image src="../../static/images/my/phone.png" style="width: 32upx;height:42upx;"></image>
						</view>
					</view>
				</view>
			</view> -->
			<view class="bg padding radius margin-top">
				<view class="text-lg text-bold">
					服务信息
				</view>
				<view class=" margin-right-xs">
					<view class="flex justify-between margin-top-lg" style="font-weight: bold;color: red;" v-if="order.state == 1">
						<view class="" style="width: 150upx;color: #999999;">确认码</view>
						<view>{{order.code}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">服务时间</view>
						<view>{{order.startTime}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">停靠位置</view>
						<view>{{order.detailsAddress}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.carNo">
						<view class="" style="width: 150upx;color: #999999;">车 牌 号</view>
						<view class="text-white">{{order.carNo}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.carNo">
						<view class="" style="width: 150upx;color: #999999;">车辆型号</view>
						<view class="text-white">{{order.carType}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.carNo">
						<view class="" style="width: 150upx;color: #999999;">车辆颜色</view>
						<view class="text-white">{{order.carColor}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">车主姓名</view>
						<view class="text-white">{{order.carName}}</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">联系电话</view>
						<view class="text-white">{{order.carPhone}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.laundryName">
						<view class="" style="width: 150upx;color: #999999;">站点名称</view>
						<view class="text-white">{{order.laundryName}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.cardName">
						<view class="" style="width: 150upx;color: #999999;">优惠卡</view>
						<view class="text-white">{{order.cardName}}</view>
					</view>
					<view class=" margin-top-lg">
						<view class="" style="width: 165rpx;color: #999999;">备注</view>
						<view class="text-white margin-top">{{order.remarks?order.remarks:'暂无'}}</view>
					</view>
				</view>
			</view>

			<view class="bg padding radius margin-top" v-if="(order.state==1||order.state==2||order.state==6)&& order.startImg">
				<view class="text-lg text-bold margin-bottom">服务前照片</view>
				<!-- <view>
					<swiper class="screen-swiper" style="height: 260rpx;" :circular="true" :autoplay="true"
						duration="800">
						<swiper-item v-for="(item,index) in order.startImg" :key="index"
							@click="saveImg( order.startImg,index)">
							<image :src="item" mode="aspectFit" class="radius"></image>
						</swiper-item>
					</swiper>
				</view> -->
				<view class="flex flex-wrap justify-between">
					<view class="flex margin-bottom-sm"
						style="width: 48%;height: 200rpx;margin-right: 5rpx;position: relative;"
						v-for="(image,index) in order.startImg" :key="index" @click="saveImg( order.startImg,index)">
						<image :src="image" style="width: 100%;height: 100%;border-radius: 10upx;" mode="aspectFill">
						</image>
					</view>
				</view>
			</view>
			<view class="bg padding radius margin-top" v-if="(order.state==2||order.state==6)&& order.endImg">
				<view class="text-lg text-bold margin-bottom">服务后照片</view>
				<!-- <view>
					<swiper class="screen-swiper" style="height: 260rpx;" :circular="true" :autoplay="true"
						duration="800">
						<swiper-item v-for="(item,index) in order.startImg" :key="index"
							@click="saveImg( order.startImg,index)">
							<image :src="item" mode="aspectFit" class="radius"></image>
						</swiper-item>
					</swiper>
				</view> -->
				<view class="flex flex-wrap justify-between">
					<view class="flex margin-bottom-sm"
						style="width: 48%;height: 200rpx;margin-right: 5rpx;position: relative;"
						v-for="(image,index) in order.endImg" :key="index" @click="saveImg( order.endImg,index)">
						<image :src="image" style="width: 100%;height: 100%;border-radius: 10upx;" mode="aspectFill">
						</image>
					</view>
				</view>
			</view>
			<view class="bg padding radius margin-top" style="margin-bottom: 140rpx;">
				<view class="text-lg text-bold">
					订单信息
				</view>
				<view class="margin-right-xs">
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">订单编号</view>
						<view class="text-white flex align-center">{{order.ordersNo}}
							<image src="../static/copy.png" style="width: 45rpx;height: 45rpx;margin-left: 5upx;"
								@click.stop="copyClick(order.ordersNo)"></image>
						</view>
					</view>
					<view class="flex justify-between margin-top-lg">
						<view class="" style="width: 150upx;color: #999999;">下单时间</view>
						<view class="text-white">{{order.createTime}}</view>
					</view>
					<view class="flex justify-between margin-top-lg" v-if="order.couponMoney">
						<view class="" style="width: 150upx;color: #999999;">优惠金额</view>
						<view class="text-white">-￥{{order.couponMoney}}</view>
					</view>
				</view>
			</view>

			<view class="box" v-if="showPay">
				<view class="bottbox">
					<view class="flex align-start justify-between">
						<view
							style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
							选择支付方式
						</view>
						<view class="margin-right margin-top-sm" @click="bindclost">
							<image src="../static/colse.png" style="width: 60upx;height: 60upx;"></image>
						</view>
					</view>
					<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
						v-for="(item,index) in openLists" :key='index'>
						<view style="display: flex;width:80%;align-items: center;">
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
							</view>
						</view>
						<view style="width: 20%;display: flex;justify-content: flex-end;">
							<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
								<label class="tui-radio">
									<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
					<view
						style="width: 690rpx;height: 80rpx;background:#1789FD;color:#FFFFFF;text-align: center;line-height: 80rpx;border-radius: 50rpx;margin: 30rpx;"
						@click="pay()">确认支付</view>
				</view>

			</view>
			<!-- <u-popup v-model="showPay" mode="bottom" border-radius="14" :closeable="true">
				<view>
					<view
						style="width: 100%;text-align: center;font-size:38rpx;font-weight: bold;margin-top:15rpx;margin-bottom: 80rpx;">
						选择支付方式
					</view>
					<view style="display: flex;height: 100upx;align-items: center;padding: 20upx 30rpx;"
						v-for="(item,index) in openLists" :key='index'>
						<view style="display: flex;width:80%;align-items: center;">
							<image :src="item.image" style="width: 55upx;height: 55upx;border-radius: 50upx;"></image>
							<view style="font-size: 30upx;margin-left: 20upx;width: 70%;">{{item.text}}
							</view>
						</view>
						<view style="width: 20%;display: flex;justify-content: flex-end;">
							<radio-group name="openWay" style="margin-left: 20upx;" @tap='selectWay(item.id)'>
								<label class="tui-radio">
									<radio color="#1789FD" :checked="openWay === item.id ? true : false" />
								</label>
							</radio-group>
						</view>
					</view>
					<view
						style="width: 690rpx;height: 80rpx;background:#1789FD;color:#FFFFFF;text-align: center;line-height: 80rpx;border-radius: 50rpx;margin: 30rpx;"
						@click="pay()">确认支付</view>
				</view>
			</u-popup>
 -->
			<view class="flex tabber padding-top-sm padding-bottom-sm align-center"
				v-if="order.state == 0||order.state == 1">
				<u-button @click="cancelOrder(order)" shape="circle" class="margin-right" :custom-style="customStyle"
					:hair-line="false" v-if="order.state == 0">取消订单
				</u-button>
				<!-- <view class="btn" @click="openBox()" v-if="order.state == 0">立即支付</view> -->
				<u-button class="margin-right" shape="circle" :custom-style="customStyle2" :hair-line="false"
					@click="showPay=true" v-if="order.state == 0">立即支付
				</u-button>
			<!-- 	<u-button v-if="order.state == 3" class="margin-right" :custom-style="customStyle" shape="circle"
					:plain="true" @click="delOrder(order)">删除订单</u-button> -->
				<u-button v-if="order.state == 6" class="margin-right" :custom-style="customStyle" shape="circle"
					:plain="true" @click="cancel(item)"> 确认完成</u-button>
				<u-button v-if="order.state == 1" class="margin-right" :custom-style="customStyle" shape="circle"
					:plain="true" @click="cancel(item)"> 订单完成</u-button>
			</view>
		</view>

	</view>
</template>

<script>
	export default {
		data() {
			return {
				customStyle: {
					backgroundColor: '#557EFD',
					color: '#FFFFFF',
					width: "200upx",
					margin: "0 30upx 0upx 0"
				},
				customStyle2: {
					backgroundColor: '#557EFD',
					color: '#FFFFFF',

					width: "200upx",
					margin: "0 30upx 0upx 0"
				},
				id: '',
				order: {
					user: {},
					game: {}
				},
				isTrue: 0,

				phone: '',
				isVip: false,
				showPay: false,
				openWay: 0,
				openLists: [],

			}
		},
		onLoad(e) {
			this.isTrue = e.isTrue
			if (this.isTrue) {
				uni.setNavigationBarTitle({
					title: '订单详情'
				})
			}
			this.id = e.id
			// #ifdef APP-PLUS
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 2;
			// #endif

			// #ifdef H5
			this.openLists = [{
				image: '../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 1;
			// #endif
			// this.getOrder()
		},
		onShow() {
			this.showPay = false
			this.getOrder()
			this.token = uni.getStorageSync('token')
			if (uni.getStorageSync('token')) {
				this.getIsVip()
			}
		},
		methods: {
			bindclost() {
				this.showPay = false
			},
			// 查看图片
			saveImg(imgs, index) {
				if (this.LBSelect) {
					if (index == this.LBIndex - 1) {
						return;
					}
				}
				// console.log(imgs)
				let that = this;
				let imgArr = imgs
				// imgArr.push(imgs);
				// //预览图片
				uni.previewImage({
					urls: imgArr,
					current: imgArr[index]
				});
			},
			copyClick(copy) {
				uni.setClipboardData({
					data: copy,
					success: function(res) {
						uni.getClipboardData({
							success: function(res) {
								uni.showToast({
									title: "复制成功",
									icon: 'none',
								});
							},
						});
					},
				});
			},
			// 修改订单地址
			undateAdd(order) {
				if (order.state == 0) {
					uni.navigateTo({
						url: '../address/address?order=' + order.ordersId + '&id=' + 3
					})
				}
			},
			// 一键导航
			bindGps(latitude, longitude, name) {
				uni.openLocation({
					latitude: Number(latitude), //要去的纬度-地址       
					longitude: Number(longitude), //要去的经度-地址
					name: name, //地址名称
					address: name, //详细地址名称
					success: function() {
						console.log('导航成功');
					},
					fail: function(error) {
						console.log(error)
					}
				});
			},
			// 拨打电话
			bindphone(phone) {
				let that = this

				uni.showModal({
					title: '提示',
					content: '是否拨打电话',
					success: function(res) {
						if (res.confirm) {
							console.log('用户点击确定', that.phone);
							uni.makePhoneCall({
								phoneNumber: phone //仅为示例
							});
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			binddetail(e) {
				console.log(e)
				uni.navigateTo({

					url: '/pages/index/game/order?id=' + e.orderTakingId
				})
			},
			getOrder() {
				let data = {
					id: this.id
				}
				this.$Request.get('/app/orders/queryOrders', data).then(res => {
					if (res.code == 0) {
						this.order = res.data
						if (this.order.orderTaking.gameId) {
							this.order.orderTaking.gameId = this.order.orderTaking.gameId.split(",");
						}
						if (this.order.startImg) {
							this.order.startImg = this.order.startImg.split(",");
						}
						if (this.order.endImg) {
							this.order.endImg = this.order.endImg.split(",");
						}
						if (this.order.carPhone) {
							let carPhone = this.order.carPhone
							this.order.carPhone = carPhone.substring(0, 3) + "****" + carPhone.substr(carPhone
								.length - 4);
						}
						console.log(this.order.carPhone, 'vipvipvipv')
					}
				})
			},
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
									// that.mescroll.resetUpScroll()
									uni.navigateBack()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			// 支付订单
			pay() {
				let that = this
				that.showPay = false;
				uni.showModal({
					title: '付款提示',
					content: '确认支付' + that.order.payMoney + '元吗?',
					success: function(re) {
						if (re.confirm) {
							that.$queue.showLoading('支付中...')
							//classify  1app  2公众号 3小程序
							let classify = 1;
							if (that.openWay == 2) { //微信
								// #ifdef MP-WEIXIN
								that.$Request.post("/app/wxPay/wxPayOrder", {
									orderId: that.order.ordersId,
									classify: 3
								}).then(red => {
									if (red.code == 0) {
										uni.hideLoading();
										uni.requestPayment({
											provider: 'wxpay',
											timeStamp: red.data.timestamp,
											nonceStr: red.data.noncestr,
											package: red.data.package,
											signType: red.data.signType,
											paySign: red.data.sign,
											success: function(redd) {
												uni.removeStorageSync('EditAddress')
												uni.showLoading({
													title: '支付成功'
												});
												uni.hideLoading();
												setTimeout(function() {
													uni.navigateBack();
												}, 1000)
											},
											fail: function(err) {
												uni.hideLoading();
												that.$queue.showToast(
													'支付失败');
											}
										});
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif

								// #ifdef H5
								let ua = navigator.userAgent.toLowerCase();
								console.log(ua)
								if (ua.indexOf('micromessenger') !== -1) {
									that.$Request.post("/app/wxPay/wxPayOrder", {
										orderId: that.order.ordersId,
										classify: 4
									}).then(red => {
										uni.hideLoading();
										if (red.code == 0) {
											that.callPay(red);
										} else {
											uni.showToast({
												title: red.msg,
												icon: 'none'
											})
										}
									});
								}

								// #endif

								// #ifdef APP-PLUS
								that.$Request.post("/app/wxPay/wxPayOrder", {
									orderId: that.order.ordersId,
									classify: 1
								}).then(red => {
									uni.hideLoading();
									if (red.code == 0) {
										console.log(JSON.stringify(red))
										that.setPayment('wxpay', JSON.stringify(red.data));
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif

							} else if (that.openWay == 1) { //支付宝
								// #ifdef H5
								that.$Request.post("/app/aliPay/payOrder", {
									orderId: that.order.ordersId,
									classify: 2
								}).then(red => {
									uni.hideLoading();
									if (red.code == 0) {
										const div = document.createElement('div')
										div.innerHTML = red.data //此处form就是后台返回接收到的数据
										document.body.appendChild(div)
										document.forms[0].submit()
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
								// #ifdef APP-PLUS
								that.$Request.post("/app/aliPay/payOrder", {
									orderId: that.order.ordersId,
									classify: 1
								}).then(red => {
									uni.hideLoading();
									if (red.code == 0) {
										that.setPayment('alipay', red.data);
									} else {
										uni.showToast({
											title: red.msg,
											icon: 'none'
										})
									}
								});
								// #endif
							} else if (that.openWay == 3) { //零钱
								that.$Request.post("/app/orders/payMoney", {
									ordersId: that.order.ordersId,
								}).then(res => {
									uni.hideLoading();
									if (res.code == 0) {
										uni.showToast({
											title: '支付成功'
										})
										setTimeout(function() {
											uni.navigateBack();
										}, 1000)
									} else {
										uni.showToast({
											title: res.msg,
											icon: 'none'
										})
									}
								});
							}

						} else if (re.cancel) {
							console.log('用户点击取消');
						}
					}

				})

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
									that.mescroll.resetUpScroll()
								}
							})
						} else if (res.cancel) {
							console.log('用户点击取消');
						}
					}
				});
			},
			// 取消订单
			cancelOrder(e) {
				let data = {
					id: e.ordersId,
					status: '3'
				}
				this.$Request.get('/app/orders/cancelOrder', data).then(res => {
					if (res.code == 0) {
						uni.showToast({
							title: '取消成功',
							icon: 'none'
						})
						setTimeout(function() {
							uni.navigateBack()
						}, 1000)
					}
				})
			},
			callPay: function(response) {
				if (typeof WeixinJSBridge === "undefined") {
					if (document.addEventListener) {
						document.addEventListener('WeixinJSBridgeReady', this.onBridgeReady(response), false);
					} else if (document.attachEvent) {
						document.attachEvent('WeixinJSBridgeReady', this.onBridgeReady(response));
						document.attachEvent('onWeixinJSBridgeReady', this.onBridgeReady(response));
					}
				} else {
					this.onBridgeReady(response);
				}
			},
			onBridgeReady: function(response) {
				let that = this;
				if (!response.package) {
					return;
				}
				WeixinJSBridge.invoke(
					'getBrandWCPayRequest', {
						"appId": response.appid, //公众号名称，由商户传入
						"timeStamp": response.timestamp, //时间戳，自1970年以来的秒数
						"nonceStr": response.noncestr, //随机串
						"package": response.package,
						"signType": response.signType, //微信签名方式：
						"paySign": response.sign //微信签名
					},
					function(res) {
						if (res.err_msg === "get_brand_wcpay_request:ok") {
							// 使用以上方式判断前端返回,微信团队郑重提示：
							//res.err_msg将在用户支付成功后返回ok，但并不保证它绝对可靠。
							uni.removeStorageSync('EditAddress')
							uni.showLoading({
								title: '支付成功'
							});
							uni.hideLoading();
							setTimeout(function() {
								uni.navigateBack();
							}, 1000)
						} else {
							uni.hideLoading();
						}
						WeixinJSBridge.log(response.err_msg);
					}
				);
			},
			selectWay: function(id) {
				this.openWay = id;
			},
			setPayment(name, order) {
				let that = this;
				uni.requestPayment({
					provider: name,
					orderInfo: order, //微信、支付宝订单数据
					success: function(res) {
						uni.removeStorageSync('EditAddress')
						uni.showLoading({
							title: '支付成功'
						});
						uni.hideLoading();
						setTimeout(function() {
							uni.navigateBack();
						}, 1000)
					},
					fail: function(err) {
						uni.hideLoading();
						console.log(12)
					}
				});
			},
			goMsg() {
				let data = {
					userId: uni.getStorageSync('userId'),
					focusedUserId: this.order.user.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.user.userId == res.data.userId ? res.data.focusedUserId : this.order
							.user.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
		}
	}
</script>

<style>
	page {
		background: #f7f7f7;
	}

	.bg {
		background: #FFFFFF;
	}

	.tabber {
		width: 100%;
		background: #ffffff;
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 9;
		justify-content: flex-end;
		height: 127rpx;
		/* padding-right: 30rpx; */
	}

	.tit {
		overflow: hidden;
		text-overflow: ellipsis;
		word-wrap: break-word;
		white-space: normal;
		-webkit-line-clamp: 2;
		font-size: 30upx;
		/* font-weight: bold; */
		color: #a9a9a9;
	}

	.btn {
		background: #557EFD;
		color: #FFFFFF;
		border-radius: 50upx;
		padding: 24upx 0upx;
		text-align: center;
		width: 200upx;
		margin: 0 30upx 0upx 0;
	}

	.box {
		width: 100%;
		height:100vh;
		background: rgba(0, 0, 0, 0.7);
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		z-index: 9999;
	}

	.bottbox {
		position: fixed;
		bottom: 0upx;
		left: 0;
		right: 0;
		z-index: 9999;
		background: #ffffff;
	}
</style>
