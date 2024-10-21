<template>
	<view class="">
		<mescroll-body :sticky="true" ref="mescrollRef" @init="mescrollInit" @down="downCallback" @up="upCallback">
			
			<!-- #ifdef APP -->
			<view class="padding-lr padding-tb flex align-center justify-between u-skeleton-rect"
				style="background: #0175FE;padding-top: 100upx;" v-if="city">
				<!-- #endif -->
				<!-- #ifndef APP -->
				<view class="padding-lr padding-tb flex align-center justify-between u-skeleton-rect" v-if="city"
					style="background: #0175FE;">
					<!-- #endif -->
					<view class="flex align-center justify-between margin-right-sm" @tap.stop="goSelectCity" v-if="dingwei!='否'"
						style="line-height: 68rpx;">
						<image src="../../static/images/index/place.png" style="width: 27rpx;height: 37rpx;" class="">
						</image>
						<view class="localName margin-left-sm text-cut" style="color: #ffffff;width: 90upx;">{{city}}
						</view>
					</view>
					<!-- <u-search class="flex-sub" placeholder="搜索你需要的服务" shape="square" disabled :show-action="false"
					:animation="true" bg-color="#F7F7F7" color="#1A1A1A"></u-search> -->
					<view v-if="dingwei!='否'">
						<swiper v-if="messageList.length > 0" :autoplay="true" :vertical="true" :interval="4000"
							:circular="true" :indicator-dots="false" class="index_gonggao">
							<block v-for="(item, index) in messageList">
								<swiper-item>
									<view class="flex">
										· <image :src="item.avatar?item.avatar:'../../static/logo.png'"
											style="width: 48upx;height: 48upx;border-radius: 50upx;"></image>
										<text v-if="item.userName">{{ item.userName }}刚刚下单</text>
										<text v-else>码兄洗车上线了，快来下单吧！</text>
									</view>
								</swiper-item>
							</block>
						</swiper>
					</view>
					<view v-if="dingwei=='否'">
						<swiper v-if="messageList.length > 0" :autoplay="true" :vertical="true" :interval="4000"
							:circular="true" :indicator-dots="false" class="index_gonggao1">
							<block v-for="(item, index) in messageList">
								<swiper-item>
									<view class="flex">
										· <image :src="item.avatar?item.avatar:'../../static/logo.png'"
											style="width: 48upx;height: 48upx;border-radius: 50upx;"></image>
										<text v-if="item.userName">{{ item.userName }}刚刚下单</text>
										<text v-else>码兄洗车上线了，快来下单吧！</text>
									</view>
								</swiper-item>
							</block>
						</swiper>
					</view>
				</view>
				<!-- 服务 -->
				<view class="fw">
					<view class="fw-box">
						<swiper class="screen-swiper" style="height: 260rpx;" :circular="true" :autoplay="true"
							interval="2500" duration="800">
							<swiper-item v-for="(item,index) in swiperList" :key="index" @click="goNav(item.url)">
								<image :src="item.imageUrl" mode="aspectFit" class="radius"></image>
							</swiper-item>
						</swiper>
					</view>
				</view>
				<view class="margin padding-tb bg-white" style="border-radius: 24upx;" v-if="jingangqu!='否'">
					<u-grid :col="5" :border="false">
						<u-grid-item bg-color="#ffffff" v-for="(item,index) in gridData" :key='index'
							@click="goNav(item.url)">
							<image :src="item.imageUrl" style="width: 92rpx;height: 92rpx;border-radius: 92rpx;">
							</image>
							<view class="grid-text">{{item.name}}</view>
						</u-grid-item>
					</u-grid>
				</view>
				
				<!-- <view class=" u-skeleton-rect">
					<me-tabs v-model="tabIndex" nameKey='gameName' :tabs="tabData" @change="tabChange"></me-tabs>
				</view> -->
				<view style="background-color: #F5F5F5;padding-top: 1rpx;padding-bottom: 20rpx;margin-top:20rpx" v-if="orderList.length">
					<!-- <ren-dropdown-filter :filterData='filterData' :border="false" :defaultIndex='defaultIndex'
					@onSelected='change' class="u-skeleton-rect">
				</ren-dropdown-filter> -->
					<view class="flex justify-between margin-top-xs bg padding-sm radius margin-lr "
						v-for="(item,index) in orderList" :key='index' @click="goOrder(item)">
						<image :src="item.homepageImg?item.homepageImg: '../../static/logo.png'"
							style="width: 200rpx;height: 200rpx;border-radius: 10rpx;" mode="aspectFill"></image>
						<view class="flex-sub margin-left text-white flex flex-direction justify-between">
							<view class=" text-30"
								style="display: inline-block;width: 420rpx; overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">
								{{item.myLevel}}
							</view>
							<view class="flex-wrap flex">
								<view class="flex align-center margin-right-xs text-sm" style="color: #999999;"
									v-for="(ite,ind) in item.gameName" :key="ind">
									{{ite}}
									<view style="width: 1rpx;height: 21upx;background: #CCCCCC;margin-left: 10upx;"
										v-if="item.gameName.length-1!=ind"></view>
								</view>
							</view>
							<view class="flex" style="align-items: center;font-size: 24rpx;" v-if="item.count">
								<view style="color: #999999;background: #F2F2F2; padding: 5rpx 10rpx;">
									已服务{{item.count}}人
								</view>
							</view>

							<view style="width: 100%;display: flex;justify-content: space-between;align-items: center;">
								<view style="color:#FF1200;font-size: 31rpx;">
									￥{{isVip? item.memberMoney :item.money}}元
									<!-- /<text>{{item.unit}}</text> -->
								</view>
								<view class="text-sm"
									style="background: #D9EBFF;color: #0175FE;padding: 12rpx 38rpx;border-radius: 45rpx;">
									立即下单
								</view>
							</view>
						</view>
					</view>
				</view>
				<empty v-if="orderList.length == 0"></empty>
				<view class="bg" v-if="shipinUrl">
					<view class="margin-lr">
						<video :src="shipinUrl" style="width:100% ;height: 320upx;border-radius: 16rpx;"></video>
					</view>
				</view>
				<view class="margin-top-sm" v-if="dbannerList.length > 0">
					<view v-for="(item,index) in dbannerList" @click="goNav(item.url)">
						<image :src="item.imageUrl" mode="widthFix" style="width:750upx;height:auto;">
					</view>
					</image>
				</view>
		</mescroll-body>
		<u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton>
		<image src="/static/images/kefu.png" @tap="Changekefu" style="width: 120rpx;height: 120rpx;position: fixed;right: 30rpx;bottom: 200rpx;"></image>
		<!-- 新人红包 -->
		<uni-popup ref="popushongbao" type="center">
			<view>
				<image @tap="GetQuan"
					src="https://xichewap.xianmxkj.com/file/uploadPath/2022/11/28/f9e36ad51b454baea28691fcb8416d05.png"
					style="width: 564upx;height:618upx "></image>
			</view>
		</uni-popup>
	</view>
</template>

<script>
	import MescrollMixin from "@/components/mescroll-uni/components/mescroll-uni/mescroll-mixins.js";
	import mescrollBody from "@/components/mescroll-uni/components/mescroll-body/mescroll-body.vue";
	import meTabs from "@/components/mescroll-uni/me-tabs/me-tabs.vue";
	import empty from '@/components/empty.vue'
	import upopup from '@/components/uni-popup/uni-popup.vue';
	import RenDropdownFilter from '@/components/ren-dropdown-filter/ren-dropdown-filter.vue'

	export default {
		mixins: [MescrollMixin], // 使用mixin
		components: {
			mescrollBody,
			upopup,
			meTabs,
			empty,
			RenDropdownFilter
		},
		onShareAppMessage(res) {
			return {
				path: '/pages/index/index?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		/*
		 * uniapp微信小程序分享页面到微信朋友圈
		 */
		onShareTimeline(res) {
			return {
				path: '/pages/index/index?invitation=' + this.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.tuiName,
				imageUrl: this.tuiImage
			}
		},
		data() {
			return {
				// popushongbao: false,
				shipinUrl:'',
				dbannerList: [],
				tuiName: '',
				tuiImage: '',
				loading: true, // 是否显示骨架屏组件
				defaultSelected: [],
				tabIndex: 0, // tab下标
				tabData: [{
					createTime: "",
					gameName: '服务推荐',
					gameImg: "",
					id: 0,
					status: 0,
					updateTime: "",
				}],
				swiperList: [],
				gridData: [],
				messageList: [{
					id: 1,
					title: 'sasa'
				}],
				value1: '',
				value2: '',
				value3: '',
				game: [],
				defaultIndex: [0, 0, 0],
				filterData: [
					[{
							label: '智能优选',
							value: '',
						},
						{
							label: '距离优先',
							value: 3,
						},
						{
							label: '人气优先',
							value: 2,
						},
						{
							label: '同城',
							value: 1,
						}
					],
					[{
							label: '销量',
							value: '',
						},
						{
							label: '从高到低',
							value: 'desc',
						},
						{
							label: '从低到高',
							value: 'asc',
						}
					],
					[{
							label: '价格',
							value: '',
						},
						{
							label: '从高到低',
							value: 'desc',
						},
						{
							label: '从低到高',
							value: 'asc',
						}
					],
				],

				city: '请选择城市',
				latitude: '',
				longitude: '',
				orderList: [],
				token: '',
				XCXIsSelect: '否',
				isVip: false,
				myId: uni.getStorageSync('userId') ? uni.getStorageSync('userId') : '',
				showModal: true,
				arr: [],
				geRen: 0,
				Qe: 0,
				renzheng: false,
				invitationCode: '',
				jingangqu:'否',
				dingwei:'否'
			}
		},
		onLoad(e) {
			let that = this
			that.getClassfly()

			uni.getLocation({
				type: 'gcj02', //wgs84  gcj02
				success: function(res) {
					console.log(res, '地理位置')
					
					that.latitude = res.latitude
					that.longitude = res.longitude
					uni.setStorageSync('latitude', res.latitude)
					uni.setStorageSync('longitude', res.longitude)
					
					
					
					// #ifdef MP-WEIXIN
					let key = that.$queue.getData('key');
					uni.request({
						url: 'https://apis.map.qq.com/ws/geocoder/v1/?location=' + that.latitude +
							',' + that.longitude + '&key='+key,
						success(re) {
							console.log(re)
							if (re.statusCode === 200) {
								let citydata = re.data.result.address_component.district
								console.log("获取城市名称成功", citydata)
								that.city = citydata ? citydata : '未知'
								uni.setStorageSync('city', citydata)
								let data = {
									num: 1,
									size: 10
								}
								that.getData(data)
							} else {
								console.log("获取信息失败，请重试！")
							}
						}
					});
					// #endif

					// #ifdef APP-PLUS

					if (res.address && res.address.city) {
						that.city = res.address.city
						uni.setStorageSync('city', res.address.city)
					} else {
						that.selectCity(that.longitude, that.latitude);
					}

					let data = {
						num: 1,
						size: 10
					}
					that.getData(data)

					// #endif

					// #ifdef H5
					that.selectCity(that.longitude, that.latitude);
					// #endif


				},
				fail: function() {
					console.log('获取地址失败')
				}
			})

			// 获取邀请码保存到本地
			if (e.invitation) {
				that.$queue.setData('inviterCode', e.invitation);
			}
            
            // #ifdef MP-WEIXIN
			if (e.scene) {
				const scene = decodeURIComponent(e.scene);
				that.$queue.setData('inviterCode', scene.split(',')[0]);
			}
			// #endif
            
			if (this.myId) {
				that.$Request.getT('/app/common/type/276').then(res => { //分享标题 276
					if (res.code == 0) {
						that.tuiName = res.data.value
					}
				})
				that.$Request.getT('/app/common/type/277').then(res => { //分享图 277
					if (res.code == 0) {
						that.tuiImage = res.data.value
					}
				})
			}
			// this.Qe = uni.getStorageSync("Qe")
			// this.geRen = uni.getStorageSync("geRen")
		},

		onShow() {
			let that = this
			that.XCXIsSelect = that.$queue.getData('XCXIsSelect');
			that.jingangqu = that.$queue.getData('jingangqu');
			
			that.invitationCode = that.$queue.getData('invitationCode');
			that.Qe = uni.getStorageSync("Qe")
			that.geRen = uni.getStorageSync("geRen")

			if (that.Qe == 2 || that.geRen == 2) {
				this.renzheng = false
			} else {
				this.renzheng = true
			}
			that.city = uni.getStorageSync('city') ? uni.getStorageSync('city') : '请选择城市'
			let data = {
				num: 1,
				size: 10
			}
			that.getData(data)


			that.token = uni.getStorageSync('token')
			if (uni.getStorageSync('token')) {
				that.getIsVip()
				
				//新人优惠券 280
				that.$Request.get('/app/common/type/280').then(res => {
					// console.log(res.data.value,'--------',res.data.value!=''&&res.data.value!='null')
					if (res.code == 0&&(res.data.value!=''&& res.data.value!='null' )) {
						that.checkNewUser();
					}
				});
			}
			that.$Request.getT('/app/common/type/310').then(res => { //是否开启定位 310
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.dingwei = res.data.value
					}
			
				}
			})
			that.myId = uni.getStorageSync('userId')
			that.$Request.getT('/app/common/type/316').then(res => { //首页视频
				if (res.code == 0) {
					if (res.data && res.data.value) {
						that.shipinUrl = res.data.value;
					}
				}
			})
			
			that.$Request.get("/app/banner/selectBannerList", {
				classify: 6
			}).then(res => {
				if (res.code == 0) {
					that.dbannerList = res.data
				}
			});
			
			// console.log(that.showModal, '------', that.myId)
			// #ifdef MP-WEIXIN
			//订阅
			if (that.myId) {
				that.$Request.getT('/app/common/type/278').then(res => { //订单变更通知 278
					if (res.code == 0) {
						if (res.data && res.data.value) {
							that.arr.push(res.data.value)
						}

					}
				})
				
				if (that.showModal) {
					that.openMsg()
				}
			}
			// #endif

		},
		methods: {
			Changekefu() {
				let that = this
				let kefu = this.$queue.getData('kefu'); // 用户端联系方式 1 手机号 2企业微信 3 客服二维码
				let kefuPhone = this.$queue.getData('kefuPhone');
				if (kefu == 1) {
					uni.makePhoneCall({
						phoneNumber: kefuPhone //仅为示例
					});
				} else if (kefu == 3) { //客服二维码
					uni.navigateTo({
						url: '/my/setting/customer'
					});
				} else {
					// #ifdef MP-WEIXIN
					let that = this
					try {
						wx.openCustomerServiceChat({
							extInfo: {
								url: that.$queue.getData('kefuUrl')
							},
							corpId: that.$queue.getData('kefuAppId'),
							success(res) {},
							fail(res) {
								console.error(res)
							}
						})
					} catch (error) {
						console.error("catchcatch" + error)
						uni.showToast({
							title: '请更新至微信最新版本'
						});
					}
					// #endif
					// #ifndef MP-WEIXIN
					let url = this.$queue.getData('kefuUrl');
					if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1) {
						uni.navigateTo({
							url
						});
					} else {
						//#ifndef H5
						uni.navigateTo({
							url: '/pages/index/webView?url=' + url
						});
						//#endif
						//#ifdef H5
						window.location.href = url;
						//#endif
					}
					// #endif
				}
			
			
			},
			GetQuan() {
				let userId = this.$queue.getData('userId');
				this.$Request.get('/app/coupon/insertNewUserCoupon').then(res => {
					if (res.code == 0) {
						// this.popushongbao = false;
						this.$refs.popushongbao.close();
						this.$queue.showToast('领取成功！')
						setTimeout(d => {
							uni.navigateTo({
								url: '/my/youhuijuan/youhui'
							});
						}, 1000);
					} else {
						this.$queue.showToast(res.msg)
					}
				});
			},
			checkNewUser() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						// this.popushongbao = true;
						if(res.data.isCoupon!=1){
							this.$refs.popushongbao.open();
						}
					}
				});
			},
			getNewOrder() {
				this.$Request.get('/app/orders/selectNewestOrders').then(res => {
					if (res.code == 0) {
						this.messageList = res.data
					}
				});

			},
			selectCity(longitude, latitude) {
				this.$Request.get('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					if (res.code == 0) {
						console.log(JSON.stringify(res))
						this.city = res.data.district ? res.data.district : '未知'
						uni.setStorageSync('city', res.data.district)
						let data = {
							num: 1,
							size: 10
						}
						this.getData(data)
					}
				});
			},
			// 开启订阅消息
			openMsg() {
				console.log('订阅消息')
				var that = this
				uni.getSetting({
					withSubscriptions: true, //是否获取用户订阅消息的订阅状态，默认false不返回
					success(ret) {
						console.log(ret.subscriptionsSetting, '------------------')
						// if (ret.subscriptionsSetting.itemSettings && Object.keys(ret.subscriptionsSetting.itemSettings).length == 2) {
						if (ret.subscriptionsSetting.itemSettings) {
							uni.setStorageSync('sendMsg', true)
							uni.openSetting({ // 打开设置页 
								success(rea) {
									console.log(rea.authSetting)
								}
							});
						} else { // 用户没有点击“总是保持以上，不再询问”则每次都会调起订阅消息
							console.log(99999)
							uni.setStorageSync('sendMsg', false)
							uni.showModal({
								title: '提示',
								content: '为了更好的体验,请绑定消息推送',
								confirmText: '确定',
								cancelText: '取消',
								success: function(res) {
									if (res.confirm) {
										wx.requestSubscribeMessage({
											tmplIds: that.arr,
											success(re) {
												console.log(JSON.stringify(re),
													'++++++++++++++')
												var datas = JSON.stringify(re);
												if (datas.indexOf("accept") != -1) {
													console.log(re)
													// uni.setStorageSync('sendMsg', true)
												}
											},
											fail: (res) => {
												console.log(res)
											}
										})
										// uni.setStorageSync('sendMsg', true)
										console.log('确认')
										that.showModal = false
									} else if (res.cancel) {
										console.log('取消')
										// uni.setStorageSync('sendMsg', false)
										that.showModal = true
									}
								}
							})
						}
					}
				})
			},
			getIsVip() {
				this.$Request.get("/app/UserVip/isUserVip").then(res => {
					if (res.code == 0) {
						this.isVip = res.data
						uni.setStorageSync('isVIP', res.data)
					}
				});
			},
			/*下拉刷新的回调 */
			downCallback() {
				// 这里加载你想下拉刷新的数据, 比如刷新轮播数据
				// loadSwiper();
				// 下拉刷新的回调,默认重置上拉加载列表为第一页 (自动执行 page.num=1, 再触发upCallback方法 )
				// this.$refs.uDropdown.close();
				this.mescroll.resetUpScroll()
			},
			/*上拉加载的回调: 其中page.num:当前页 从1开始, page.size:每页数据条数,默认10 */
			upCallback(page) {
				// console.log(page,'*************')
				this.getData(page)
			},
			getData(page) {
				let curTab = this.tabData[this.tabIndex].gameName
				if (curTab == '服务推荐') {
					curTab = ''

				} else if (curTab != '服务推荐') {
					curTab = this.tabData[this.tabIndex].gameName
				}
				let num
				if (this.tabIndex == 0) {
					num = 0
				} else {
					num = 2
				}
				let data = {
					id: curTab,
					page: page.num,
					limit: page.size,
					isRecommend: num,
					condition: this.value1, //智能优选
					salesNum: this.value2, //不限男女
					by: this.value3, //价格
					latitude: this.latitude,
					longitude: this.longitude,
					city: this.city
				}
				// console.log(data)
				if (this.token) {
					this.$Request.get("/app/orderTaking/queryTaking", data).then(res => {
						this.mescroll.endBySize(res.data.list.length, res.data.list)
						if (res.code == 0) {
							if (page.num == 1) {
								for (let i = 0; i < res.data.list.length; i++) {
									if(res.data.list[i].gameName){
										res.data.list[i].gameName = res.data.list[i].gameName.split(",");
									}
								}
								this.orderList = res.data.list
							} else {
								for (let i = 0; i < res.data.list.length; i++) {
									if(res.data.list[i].gameName){
										res.data.list[i].gameName = res.data.list[i].gameName.split(",");
									}
								}
								this.orderList = [...this.orderList, ...res.data.list]
							}
							this.loading = false;
							this.getBannerList()
							this.getGrid()
							this.getNewOrder()
						}
						this.mescroll.endSuccess(res.data.list.length); // 隐藏加载状态栏
					}).catch(() => {
						//联网失败, 结束加载
						this.mescroll.endErr();
					});
				} else {
					this.$Request.get("/app/orderTaking/queryTakings", data).then(res => {
						this.mescroll.endBySize(res.data.list.length, res.data.list)
						this.loading = false;
						if (res.code == 0) {
							if (page.num == 1) {
								this.orderList = res.data.list
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");
								}
							} else {
								this.orderList = [...this.orderList, ...res.data.list]
								for (let i = 0; i < this.orderList.length; i++) {
									this.orderList[i].gameName = this.orderList[i].gameName.split(",");
								}
							}

						}
						this.mescroll.endSuccess(res.data.list.length); // 隐藏加载状态栏
					}).catch(() => {
						//联网失败, 结束加载
						this.mescroll.endErr();
					});
				}
				this.getClassfly()
				this.getBannerList()
				this.getGrid()

			},
			// 切换菜单
			tabChange() {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								// console.log(re)
							}
						},
						fail: (res) => {
							// console.log(res)
						}
					})
				}
				this.defaultIndex = [0, 0, 0]
				// this.$refs.uDropdown.close();
				// this.orderList = []; // 置空列表,显示加载进度条
				this.mescroll.resetUpScroll()
			},
			// 获取游戏类型
			getClassfly() {
				this.$Request.get("/app/appGame/queryGameName").then(res => {
					if (res.code == 0) {
						this.tabData = [{
							createTime: "",
							gameName: '服务推荐',
							gameImg: "",
							id: 0,
							status: 0,
							updateTime: "",
						}]
						this.tabData = [...this.tabData, ...res.data]

					}
				});
			},
			//获取轮播图
			getBannerList() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 1
				}).then(res => {
					if (res.code == 0) {
						this.swiperList = res.data
					}
				});
			},
			// 获取金刚区分类
			getGrid() {
				this.$Request.get("/app/banner/selectBannerList", {
					classify: 2
				}).then(res => {
					if (res.code == 0) {
						this.gridData = res.data
						// console.log(this.gridData, '；；；；；；')
					}
				});
			},

			// 筛选
			change(e) {

				this.value1 = e[0][0].value
				this.value2 = e[1][0].value
				this.value3 = e[2][0].value

				this.mescroll.resetUpScroll()
			},
			// 选择城市
			goSelectCity() {
				// uni.navigateTo({
				// 	url: '/pages/index/citys/citys'
				// });
				
				const that = this;
				uni.chooseLocation({
					success: function(res) {
						console.log(res)
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						if (res.name && res.address) {
							that.latitude = res.latitude
							that.longitude = res.longitude
							that.selectCity(that.longitude, that.latitude);
							// uni.setStorageSync('latitude', res.latitude)
							// uni.setStorageSync('longitude', res.longitude)
							// that.city = res.data.district ? res.data.district : '未知'
							// uni.setStorageSync('city', res.data.district)
						} else {
							that.$queue.showToast('请选择正确地址信息！');
						}
					}
				});
			},

			// 跳转游戏列表
			goNav(url) {
				console.log(url, '1111112333')
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (url.indexOf('/pages/') !== -1 || url.indexOf('/my/') !== -1) {
					uni.navigateTo({
						url
					});
				} else {
					//#ifndef H5
					uni.navigateTo({
						url: '/pages/index/webView?url=' + url
					});
					//#endif
					//#ifdef H5
					window.location.href = url;
					//#endif
				}
			},
			// 跳转搜索
			goSearch() {
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							// console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				uni.navigateTo({
					url: '/pages/index/search/index'
				});
			},
			// 跳转订单
			goOrder(e) {
				console.log('授权', uni.getStorageSync('sendMsg'))
				if (uni.getStorageSync('sendMsg')) {
					console.log('授权+1')
					wx.requestSubscribeMessage({
						tmplIds: this.arr,
						success(re) {
							console.log(JSON.stringify(re), 111111111111)
							var datas = JSON.stringify(re);
							if (datas.indexOf("accept") != -1) {
								console.log(re)
							}
						},
						fail: (res) => {
							console.log(res)
						}
					})
				}
				if (this.token) {
					uni.navigateTo({
						url: '/pages/index/game/order?id=' + e.id
					});
				} else {
					uni.navigateTo({
						url: '/pages/public/login'
					});
				}

			},
		}
	}
</script>

<style lang="scss">
	page {
		background-color: #F7F7F7;
	}

	.bg {
		background: #FFFFFF;
	}

	.fw {
		width: 100%;
		// height: 180rpx;
		background: linear-gradient(to bottom, #0175FE 5%, #F7F7F7 90%);
		display: flex;
		justify-content: center;
		align-items: flex-end;

		.fw-box {
			width: 686rpx;
			// height: 160rpx;
			// background-color: #ffffff;
			border-radius: 24rpx;
		}
	}

	.sticky-tabs {
		z-index: 990;
		position: sticky;
		top: var(--window-top);
		// background-color: #fff;
	}

	/* // 使用mescroll-uni,则top为0 */
	.mescroll-uni,
	/deep/.mescroll-uni {
		.sticky-tabs {
			top: 0;
		}
	}

	.demo-tip {
		padding: 18upx;
		font-size: 24upx;
		text-align: center;
	}

	.line_s {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 10rpx;
		height: 10rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.index_gonggao {
		color: #FDFFFE;
		width: 520rpx;
		height: 68rpx;
		background: #EAF4FE;
		border-radius: 50rpx;
		align-items: center;
		line-height: 50rpx;
		padding: 10rpx 15rpx;

		text {
			height: 80rpx;
			font-size: 26rpx;
			font-family: PingFang SC Medium, PingFang SC Medium-Medium;
			font-weight: 500;
			margin-left: 10rpx;
			color: #0175FE;
		}
	}
	.index_gonggao1{
		color: #FDFFFE;
		width: 690rpx;
		height: 68rpx;
		background: #EAF4FE;
		border-radius: 50rpx;
		align-items: center;
		line-height: 50rpx;
		padding: 10rpx 15rpx;

		text {
			height: 80rpx;
			font-size: 26rpx;
			font-family: PingFang SC Medium, PingFang SC Medium-Medium;
			font-weight: 500;
			margin-left: 10rpx;
			color: #0175FE;
		}
	}
</style>
