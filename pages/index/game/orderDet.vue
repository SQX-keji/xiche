<template>
	<view style="padding-bottom: 80px;">
		<view class="padding">
			<view class=" bg radius u-skeleton-fillet  " style="padding: 25rpx;">
				<view class="flex" style="position: relative;">
					<image :src="order.homepageImg" style="width: 160upx;height: 160upx;border-radius: 8upx;"
						mode="aspectFill"></image>
					<view class="u-flex-1 text-white margin-left-xs">
						<view class="u-font-16  text-bold u-line-1" style="width: 480rpx;">{{order.myLevel}}</view>
						<view class="margin-tb-sm flex align-center text-sm">
							<view class="flex align-center margin-right-xs" style="color: #999999;"
								v-for="(item,index) in order.gameName" :key="index">
								{{item}}
								<view style="width: 1rpx;height: 21upx;background: #CCCCCC;margin-left: 10upx;"
									v-if="order.gameName.length-1!=index"></view>
							</view>
						</view>
						<!-- <view style="color: #FF2D01;font-size: 32upx;">
							<text class="text-sm">￥</text>{{isVip? order.memberMoney :order.money}}
						</view> -->

						<view class="text-xxl flex align-center" style="color: #FF2D01;font-size: 32upx;"
							v-if="order.memberMoney||order.money">
							<view class=""><text class="text-df ">￥</text>{{order.money}}
							</view>
							<view style="font-size: 24rpx;color: #999999;margin-left: 10rpx;color: #FF2D01;"
								class="text-df">会员尊享￥<text style="color: #FF2D01;">{{order.memberMoney}}</text></view>
						</view>
					</view>

				</view>
			</view>
			<view class="padding bg margin-top radius u-skeleton-fillet">
				<view class="flex align-center justify-between padding-tb-xs" @click="bindmap">
					<view class="text-30" style="color: #1E1F31;width: 200upx;">车辆位置</view>
					<view class="flex justify-between" style="width: 80%;">
						<view class="margin-right-xs " style="width: 90%;text-align: right;">
							{{carAdd?carAdd:'请选择车辆停靠位置'}}
						</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<!-- @click="openzhandian" -->
				<view class="flex align-center justify-between padding-tb-xs">
					<view class="text-30" style="color: #1E1F31;width: 200upx;">附近站点</view>
					<view class="flex justify-between" style="width: 80%;">
						<view class="margin-right-xs " style="width: 90%;text-align: right;">
							{{laundryName?laundryName:'请选择附近站点'}}
						</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between ">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">停靠位置</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs ">
							<u-input v-model="address" :clearable="false" type="text" placeholder="请输入车辆停靠位置"
								placeholder-style="color:#999999;font-size:13px;" />
						</view>
						<!-- <u-icon name="arrow-right" color="#999999"></u-icon> -->
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;" v-if="isCar"></view>
				<view class="flex align-center justify-between " @click="getcar()" v-if="isCar">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">选择车辆</view>
					<view class="flex justify-between margin-tb-sm">
						<!-- <view class="margin-right-xs" >
							<u-input v-model="carNo" :disabled="true" :clearable="false" type="text" placeholder="请选择车辆"
								placeholder-style="color:#999999;font-size:13px;"
								 />
						</view> -->
						<view v-if="carNo">{{carNo}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择车辆</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between ">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">车主姓名</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs">
							<u-input v-model="carName" :clearable="false" type="text" placeholder="请输入车辆车主姓名"
								placeholder-style="color:#999999;font-size:13px;" />
						</view>
						<!-- <u-icon name="arrow-right" color="#999999"></u-icon> -->
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between ">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">联系电话</view>
					<view class="flex justify-between ">
						<view class="margin-right-xs">
							<u-input v-model="carPhone" :clearable="false" type="text" placeholder="请输入车辆联系电话"
								placeholder-style="color:#999999;font-size:13px;" />
						</view>
						<!-- <u-icon name="arrow-right" color="#999999"></u-icon> -->
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between " @click="open()">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">预约时间</view>
					<view class="flex justify-between margin-tb-sm">
						<!-- 	<view class="margin-right-xs">
							<u-input v-model="startTime" :disabled="true" :clearable="false" type="text" placeholder="请选择预约时间"
								placeholder-style="color:#999999;font-size:13px;"
								 />
						</view> -->
						<view v-if="startTime">{{startTime}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择预约时间</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"
					v-if="CouponIssueList.length > 0"></view>
				<view class="flex align-center justify-between" v-if="CouponIssueList.length > 0">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠券</view>
					<view class="flex justify-between margin-tb-sm" @click="showCoup">
						<view v-if="couponName">{{couponName}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择优惠券</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view>
				<view class="flex align-center justify-between" v-if="YouhuiList.length > 0" @tap="showYHK">
					<view class="text-30" style="color: #1E1F31;width: 240upx;">优惠卡</view>
					<view class="flex justify-between margin-tb-sm">
						<view v-if="youhuiName">{{youhuiName}}</view>
						<view class="tetx-sm" style="color:#999999;" v-else>请选择月卡/次卡</view>
						<u-icon name="arrow-right" color="#999999"></u-icon>
					</view>
				</view>
				<!-- <view class="margin-tb-sm" style="width: 100%;height: 1rpx;background: #F2F2F2;"></view> -->
				<view class="">
					<view class="text-30" style="color: #1E1F31;margin-right: 20upx;">备注说明</view>
					<textarea v-model="remark" class="text-white text-df flex-sub padding-tb-sm "
						placeholder="简单描述下您的要求" style="height: 200upx;"></textarea>
				</view>
			</view>

		</view>
		<view class="text-white flex justify-between cu-bar foot bg padding-lr" style="color: #557EFD;">
			<view>
				实付：<text>￥</text><text style="font-size: 38upx;margin-top: 8upx;">{{price}}</text>
			</view>
			<view class="">
				<u-button :custom-style="customStyle" @click="checkpay()" shape="circle" :hair-line="false">立即下单
				</u-button>
			</view>
		</view>

		<u-picker v-model="show" :defaultTime="defaulttime" mode="time" :params="params" @confirm="statusChange"></u-picker>

		<u-popup v-model="showPay" mode="bottom" border-radius="14" :closeable="true">
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
		</u-popup>
		<u-popup v-model="showKa" mode="bottom" border-radius="14" :closeable="true">
			<view
				style="width: 100%;height: 1000rpx;background: #F5F5F5;border-top-left-radius: 20upx;border-top-right-radius: 20upx;padding: 40upx;">
				<view style="width: 100%;text-align: center;font-size: 32rpx;color: #1a1a1a;">我的优惠卡</view>
				<scroll-view scroll-y="true" style="height: 900rpx;">

					<view class="listItem" v-for="(item,index) in YouhuiList">
						<view class="flex justify-between">
							<view>
								<view class="listItem_text" v-if="item.cardType == 1">{{item.cardName}}</view>
								<view class="listItem_text" style="height: 70rpx;" v-if="item.cardType == 2">
									{{item.cardName}}
								</view>
								<view class="listItem_endTime" v-if="item.cardType == 1">有效期至{{item.endTime}}</view>
							</view>
							<!-- <view class="listItem_money">¥ <text style="font-size: 48rpx;">{{item.price}}</text> </view> -->
						</view>
						<view class="listItem_xian"></view>
						<view class="flex justify-between align-center">
							<view class="listItem_guize">可使用次数：{{item.num}}</view>
							<view class="listItem_btn" @tap="goFaBus(item)">立即使用</view>
						</view>
					</view>
				</scroll-view>
			</view>
		</u-popup>
		<u-popup v-model="showCoupon" mode="bottom" border-radius="14" :closeable="false">
			<view
				style="width: 100%;height: 1000rpx;background: #FFFFFF;border-top-left-radius: 20upx;border-top-right-radius: 20upx;padding: 40upx;">
				<view class="flex justify-between align-center" style="width: 100%;margin-top: 20rpx;">
					<view style="font-size: 32rpx;color: #1a1a1a;">我的优惠券</view>
					<view @click="qingkong">
						<view class="noyouhui">不使用优惠劵</view>
					</view>
				</view>
				<scroll-view scroll-y="true" style="height: 900rpx;">
					<view style="width: 100%;text-align: center;" v-for='(item,index) in CouponIssueList' :key='index'>
						<view
							style="background: #fcf3e8;width: 100%;height: 130rpx;border-radius: 10rpx;margin-top: 20rpx;">
							<view style="display: flex;color: #1a1a1a;width: 100%;">
								<view
									style="line-height: 130rpx;margin-left: 0rpx;font-size: 40rpx;color: #1a1a1a;font-weight: 600;width: 150rpx;">
									<text style="font-size: 20upx;">￥</text>{{item.money}}
								</view>
								<view style="margin-left: 20rpx;width: 50%;text-align: left;">
									<view style="margin-top: 25rpx;">
										满{{item.minMoney}}减{{item.money}}</view>
									<view style="margin-top: 10rpx;font-size: 26rpx;">截止：{{item.expirationTime}}</view>
								</view>
								<view
									style="height: 105rpx;width: 2rpx;background: #1a1a1a;margin-left: 20rpx;margin-top: 15rpx;">
								</view>
								<view
									style="color: #1a1a1a;line-height: 130rpx;height: 130rpx;width: 145rpx;font-weight: 600;"
									@tap='goFaBu(item)'>立即使用</view>
							</view>
						</view>
					</view>
				</scroll-view>
			</view>
		</u-popup>

		<u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showKa: false,
				YouhuiList: [],
				youhuiName: '',
				youhuiId: '',
				isCar: false,
				couponName: '',
				couponId: '',
				showCoupon: false,
				CouponIssueList: [], //优惠券
				loading: true, // 是否显示骨架屏组件
				customStyle: {
					width: '340upx',
					color: '#FFFFFF',
					background: "#557EFD",
					border: 0
				},
				
				defaulttime:'',
				id: '',
				order: {},
				isVip: false,
				payPrice: '',
				price: '',
				remark: '',
				carAdd: '',
				address: '',
				carlist: [],
				carNo: '',
				carName: '',
				carPhone: '',
				show: false,
				params: {
					year: true,
					month: true,
					day: true,
					hour: true,
					minute: false,
					second: false,
					timestamp: true
				},
				startTime: '',
				longitude: '',
				latitude: '',
				startHour: '',
				showPay: false,
				openWay: 0,
				openLists: [],
				laundry: [],
				laundryName: '',
				laundryId: ''
			}
		},
		onLoad(option) {
			console.log(option)
			this.id = option.id
			uni.removeStorageSync('carlist')
			this.isVip = uni.getStorageSync('isVIP')
			this.getCheckKa();
			this.getDet()
			// #ifdef APP-PLUS
			this.openLists = [{
				image: '../../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 1;
			// #endif

			// #ifdef MP-WEIXIN
			this.openLists = [{
				image: '../../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}, {
				image: '../../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}];
			this.openWay = 2;
			// #endif

			// #ifdef H5
			this.openLists = [{
				image: '../../../static/images/zhifubao.png',
				text: '支付宝',
				id: 1
			}, {
				image: '../../../static/images/lingqian.png',
				text: '零钱',
				id: 3
			}, {
				image: '../../../static/images/icon_weixin.png',
				text: '微信',
				id: 2
			}];
			this.openWay = 1;
			// #endif
			
			let startTimeAdd = this.$queue.getData('startTimeAdd');
 			startTimeAdd = parseInt(startTimeAdd ? startTimeAdd : 0);
 			this.defaulttime = this.hourUp(startTimeAdd);
 			console.log(this.defaulttime)
		},
		destryed() {
			// uni.removeStorageSync('carlist')
		},
		onShow() {
			this.carlist = uni.getStorageSync('carlist')
			if (this.carlist) {
				this.carNo = this.carlist.carNo
				this.carName = this.carlist.carName
				this.carPhone = this.carlist.carPhone
			}

			let sitlaundryName = this.$queue.getData('sitlaundryName');
			let sitlaundryId = this.$queue.getData('sitlaundryId');
			if (sitlaundryName) {
				this.laundryName = sitlaundryName;
				this.laundryId = sitlaundryId ? sitlaundryId : '';
				this.$queue.remove('sitlaundryName');
				this.$queue.remove('sitlaundryId');
			}
		},
		methods: {
			hourUp(e) {
 				// 比现在多几个小时
 				var end = Date.now()
 				var start = 1000 * 60 * 60 * e
 				var interval = end + start; //结束 - 开始 = 毫秒值
 				var a = new Date(interval);
 				var y = a.getFullYear();
 				var m = a.getMonth() + 1;
 				m = m < 10 ? "0" + m : m;
 				var d = a.getDate();
 				d = d < 10 ? "0" + d : d;
 
 				var h = a.getHours();
 				h = h < 10 ? "0" + h : h;
 				var mm = a.getMinutes();
 				mm = mm < 10 ? "0" + mm : mm;
 				var s = a.getSeconds();
 				s = s < 10 ? "0" + s : s;
 
 				var w = a.getDay() - 1;
 				return y + '-' + m + '-' + d + ' ' + h + ':00:00'
 			},
			showYHK() {
				this.showKa = true
			},
			getCheckKa() {
				this.$Request.getT('/app/myCard/selectMyCardListByOrderTakingId?page=1&limit=50&orderTakingId=' + this.id)
					.then(res => {
						if (res.code == 0) {
							this.YouhuiList = res.data.list;
						}
					});
			},
			goFaBus(item) {
				this.showKa = false;
				this.youhuiId = item.myCardId;
				this.youhuiName = '使用' + item.cardName + '进行抵扣！';
				this.couponName = '';
				this.couponId = '';
				this.price = 0;
			},
			qingkong(){
				this.showCoupon = false;
				this.couponName = '';
				this.couponId = '';
				this.youhuiId = '';
				this.youhuiName = '';
				this.price = this.payPrice;
			},
			goFaBu(item) {
				this.showCoupon = false;
				this.couponName = '￥-' + item.money;
				this.couponId = item.id;
				this.youhuiId = '';
				this.youhuiName = '';
				this.price = parseFloat(parseFloat(this.payPrice) - parseFloat(item.money)).toFixed(2);
			},
			getCouponIssueList() {

				this.$Request.getT('/app/coupon/selectUserCouponList?page=1&limit=50&minMoney=' + this.payPrice).then(
					res => {
						if (res.code == 0) {
							this.CouponIssueList = res.data.list;
						}
					});

				// let userId = this.$queue.getData('userId');
				// this.$Request.get('/app/selfCouponUser/userList?goodsId=' + goodsId + '&userId=' + userId + '&money=' + this.price + '&status=1').then(res => {
				// 	if (res.code === 0) {
				// 		res.data.forEach(d => {
				// 			this.CouponIssueList.push(d);
				// 		});
				// 	}
				// });
			},
			showCoup() {
				this.showCoupon = true;
			},
			selectWay: function(id) {
				this.openWay = id;
			},
			open() {
				this.show = true
			},
			statusChange(e) {
				let startTimeAdd = this.$queue.getData('startTimeAdd');
				startTimeAdd = parseInt(startTimeAdd ? startTimeAdd : 0);
				const date = new Date();
				let hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
				let year = date.getFullYear();
				let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
				let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
				this.startHour = hour + startTimeAdd
				if (e.year > year) {
					this.startTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':00'
				} else {
					if (e.month > month) {
						this.startTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':00'
					} else {
						if (day > e.day) {
							uni.showToast({
								title: '预约时间必须大于当前时间',
								icon: "none"
							})
							return
						}
						if (day == e.day) {
							if (e.hour < this.startHour) {
								uni.showToast({
									title: '预约时间必须大于当前时间' + startTimeAdd + '小时以上',
									icon: "none"
								})
								return
							}
						}
					}
				}

				this.startTime = e.year + '-' + e.month + '-' + e.day + ' ' + e.hour + ':00'
			},
			bindmap() {
				console.log('0000000')
				var that = this
				// if (that.ciaddress == '') {
				uni.chooseLocation({
					success: function(res) {
						// console.log(res)
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						that.carAdd = res.address + res.name
						that.latitude = res.latitude
						that.longitude = res.longitude
						that.$queue.setData('zhandianlatitude', that.latitude);
						that.$queue.setData('zhandianlongitude', that.longitude);
						that.getzhandian(res.longitude, res.latitude)
					}
				});
			},
			openzhandian() {
				if (!this.carAdd) {
					uni.showToast({
						title: '请先选择停靠位置',
						icon: 'none'
					})
					return
				}
				uni.navigateTo({
					url: '/my/zhandian/index'
				})
			},
			getzhandian(longitude, latitude) {
				this.$Request.get("/app/laundry/selectLaundryByDistance", {
					latitude: this.latitude,
					longitude: this.longitude
				}).then(res => {
					if (res.status == 0) {
						// this.laundry = res.data
						if (res.data) {
							if (res.data.distance <= parseFloat(res.data.scope * 1000)) {
								this.laundryName = res.data.laundry_name;
								this.laundryId = res.data.laundry_id;
							} else {
								this.laundryName = '';
								this.laundryId = '';
								this.$queue.showToast('当前附近' + parseFloat(res.data.scope * 1000) + '米内没有师傅可以服务！');
							}
						} else {
							this.$queue.showToast('当前附近没有师傅可以服务！');
							this.laundryName = '';
							this.laundryId = '';
						}
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			getcar() {
				console.log(8989)
				if (!this.carAdd) {
					uni.showToast({
						title: '请先选择停靠位置',
						icon: 'none'
					})
					return
				}
				uni.navigateTo({
					url: '/my/other/car'
				})
			},

			// 详情
			getDet() {
				this.$Request.get("/app/orderTaking/queryTakingDetails", {
					id: this.id
				}).then(res => {
					if (res.code == 0) {
						this.order = res.data
						this.isCar = res.data.isCar == 2 ? false : true;
						this.value = res.data.minNum
						this.minNum = res.data.minNum
						if (this.order.gameName) {
							this.order.gameName = res.data.gameName.split(',')
						}

						if (this.isVip) {
							this.price = res.data.memberMoney
							this.payPrice = res.data.memberMoney
						} else {
							this.price = res.data.money
							this.payPrice = res.data.money
						}
						this.getCouponIssueList();
						// console.log(this.price, '----------', this.isVip)
						this.loading = false;
					} else {
						uni.showToast({
							title: res.msg,
							duration: 1000,
							icon: 'none'
						});
					}
				});
			},
			checkpay() {
				if (!this.carAdd) {
					uni.showToast({
						title: '请选择您的车辆位置',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.laundryId) {
					uni.showToast({
						title: '请选择附近站点',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.address) {
					uni.showToast({
						title: '请输入车辆详细位置',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.carNo && this.isCar) {
					uni.showToast({
						title: '请选择选择车辆',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.carName) {
					uni.showToast({
						title: '请填写车主姓名',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (this.carPhone.length != 11) {
					uni.showToast({
						title: '请填写正确车主联系电话',
						icon: 'none',
						duration: 1000
					})
					return
				}
				if (!this.startTime) {
					// let startTimeAdd = this.$queue.getData('startTimeAdd');
					// startTimeAdd = parseInt(startTimeAdd ? startTimeAdd : 0);
					// const date = new Date();
					// let hour = date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
					// let year = date.getFullYear();
					// let month = date.getMonth() + 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
					// let day = date.getDate() < 10 ? "0" + date.getDate() : date.getDate();

					// this.startTime = year + '-' + month + '-' + day + ' ' + (hour + startTimeAdd) + ':00'

					uni.showToast({
						title: '请选择预约时间',
						icon: 'none',
						duration: 1000
					})
					return
				}

				if (this.youhuiName) {
					this.$queue.showLoading('提交中...');
					let that = this
					that.$Request.get("/app/orders/generateOrder", {
						couponId: that.couponId,
						id: that.order.id,
						type: 1,
						myCardId: that.youhuiId,
						city: that.carAdd,
						remarks: that.remark,
						detailsAddress: that.address,
						startTime: that.startTime,
						orderNumber: 1,
						laundryId: that.laundryId,
						carNo: that.carNo,
						carName: that.carName,
						carColor: that.carlist.carColor,
						carType: that.carlist.carType,
						carPhone: that.carPhone,
						longitude: that.longitude,
						latitude: that.latitude,
					}).then(res => {
						if (res.code == 0) {
							that.$Request.post("/app/orders/payCard", {
								ordersId: res.data.ordersId,
							}).then(res => {
								uni.hideLoading();
								if (res.code == 0) {
									uni.showToast({
										title: '抵扣成功'
									})
									setTimeout(function() {
										uni.switchTab({
											url: '/pages/order/index'
										})

									}, 1000)
									uni.removeStorageSync('EditAddress')
								} else {
									uni.showToast({
										title: res.msg,
										icon: 'none'
									})
								}
							});
						} else {
							uni.hideLoading();
							that.$queue.showToast(res.msg);
						}

					});
				} else {
					this.showPay = true
				}
			},
			pay() {
				// if (!this.remark) {
				// 	uni.showToast({
				// 		title: '请输入备注',
				// 		icon: 'none',
				// 		duration: 1000
				// 	})
				// 	return
				// }
				let that = this
				that.$Request.get("/app/orders/generateOrder", {
					couponId: that.couponId,
					id: that.order.id,
					type: 1,
					city: that.carAdd,
					myCardId: that.youhuiId,
					remarks: that.remark,
					detailsAddress: that.address,
					startTime: that.startTime,
					orderNumber: 1,
					laundryId: that.laundryId,
					carNo: that.carNo,
					carName: that.carName,
					carColor: that.carlist.carColor,
					carType: that.carlist.carType,
					carPhone: that.carPhone,
					longitude: that.longitude,
					latitude: that.latitude,
				}).then(res => {
					if (res.code == 0) {
						that.showPay = false;
						uni.showModal({
							title: '付款提示',
							content: '确认支付' + that.price + '元吗?',
							success: function(re) {
								if (re.confirm) {
									that.$queue.showLoading('支付中...')
									let classify = 1;
									if (that.openWay == 2) { //微信
										// #ifdef MP-WEIXIN
										that.$Request.post("/app/wxPay/wxPayOrder", {
											orderId: res.data.ordersId,
											classify: 3
										}).then(red => {
											uni.hideLoading();
											if (red.code == 0) {
												uni.requestPayment({
													provider: 'wxpay',
													timeStamp: red.data.timestamp,
													nonceStr: red.data.noncestr,
													package: red.data.package,
													signType: red.data.signType,
													paySign: red.data.sign,
													success: function(redd) {
														uni.hideLoading();
														uni.showToast({
															title: '支付成功'
														})
														setTimeout(function() {
															uni.switchTab({
																url: '/pages/order/index'
															})

														}, 1000)
														uni.removeStorageSync(
															'carlist')
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
												orderId: res.data.ordersId,
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
											orderId: res.data.ordersId,
											classify: 1
										}).then(red => {
											uni.hideLoading();
											if (red.code == 0) {
												console.log(JSON.stringify(red))
												that.setPayment('wxpay', JSON.stringify(red
													.data));
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
											orderId: res.data.ordersId,
											classify: 2
										}).then(red => {
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
											orderId: res.data.ordersId,
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
											ordersId: res.data.ordersId,
										}).then(res => {
											uni.hideLoading();
											if (res.code == 0) {
												uni.showToast({
													title: '支付成功'
												})
												setTimeout(function() {
													uni.switchTab({
														url: '/pages/order/index'
													})

												}, 1000)
												uni.removeStorageSync('EditAddress')
											} else {
												uni.showToast({
													title: res.msg,
													icon: 'none'
												})
											}
										});
									}
									// console.log('用户点击确定');
									// that.$Request.post("/app/orders/payMoney", {
									// 	ordersId: res.data.ordersId,
									// }).then(res => {
									// 	if (res.code == 0) {
									// 		uni.showToast({
									// 			title: '支付成功'
									// 		})
									// 		setTimeout(function() {
									// 			uni.switchTab({
									// 				url: '/pages/order/index'
									// 			})

									// 		}, 1000)

									// 		uni.removeStorageSync('carlist')
									// 	} else {
									// 		uni.showToast({
									// 			title: res.msg,
									// 			icon: 'none'
									// 		})
									// 	}
									// });
								} else if (re.cancel) {
									console.log('用户点击取消');
									// uni.removeStorageSync('carlist')
								}
							}
						})
					} else {
						that.$queue.showToast(res.msg);
					}

				});
			}
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
						uni.hideLoading();
						uni.showToast({
							title: '支付成功'
						})
						setTimeout(function() {
							uni.switchTab({
								url: '/pages/order/index'
							})

						}, 1000)
						uni.removeStorageSync('carlist')
					} else {
						uni.hideLoading();
					}
					WeixinJSBridge.log(response.err_msg);
				}
			);
		},
		setPayment(name, order) {
			let that = this;
			uni.requestPayment({
				provider: name,
				orderInfo: order, //微信、支付宝订单数据
				success: function(res) {
					uni.hideLoading();
					uni.showToast({
						title: '支付成功'
					})
					setTimeout(function() {
						uni.switchTab({
							url: '/pages/order/index'
						})

					}, 1000)
					uni.removeStorageSync('carlist')
				},
				fail: function(err) {
					uni.hideLoading();
					console.log(12)
				}
			});
		}
		// computed: {
		// 	price() {
		// 		let price = this.isVip ? this.order.memberMoney : this.order.money
		// 		console.log(price * 1 * this.value)
		// 		return (price * this.value).toFixed(2)
		// 	}
		// }
	}
</script>

<style lang="scss">
	textarea::-webkit-input-placeholder {
		color: red;

	}
	
	.noyouhui {
		color: #1a1a1a;
		border: 1rpx solid #1a1a1a;
		border-radius: 15upx;
		display: inline-block;
		padding: 5rpx 20rpx;
	}

	.listItem {
		width: 100%;
		height: fit-content;
		background: #FFFFFF;
		border-radius: 16rpx;
		margin: 20rpx 0rpx 0rpx;
		padding: 30rpx 24rpx;

		.listItem_xian {
			width: 638rpx;
			height: 2rpx;
			border: 2rpx dashed #E6E6E6;
			margin: 30rpx 0rpx 20rpx;
		}

		.listItem_guize {
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 400;
			color: #999999;
		}

		.listItem_btn {
			width: 128rpx;
			height: 48rpx;
			border: 2rpx solid #FF130A;
			border-radius: 24rpx;
			text-align: center;
			line-height: 48rpx;
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 500;
			color: #FF130A;
		}

		.listItem_text {
			font-size: 30rpx;
			font-family: PingFang SC;
			font-weight: 800;
			color: #000000;
		}

		.listItem_endTime {
			font-size: 24rpx;
			font-family: PingFang SC;
			font-weight: 400;
			color: #999999;
			margin-top: 14rpx;
		}

		.listItem_money {
			font-size: 30rpx;
			font-family: DINPro;
			font-weight: 500;
			color: #FF130A;
		}
	}

	page {
		background-color: #F5F5F5;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.u-input {
		text-align: right !important;
	}
</style>