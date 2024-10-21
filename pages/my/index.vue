<template>
	<view class="text-white ">
		<!-- #ifdef APP -->
		<view class=" u-flex margin" style="padding-top: 100upx;">
			<!-- #endif -->
			<!-- #ifndef APP -->
			<view class=" u-flex margin">
				<!-- #endif -->
				<view class="u-m-r-10">
					<image :src="avatar" style="width: 100rpx;height: 100rpx;border-radius: 100rpx;"
						@click="goNav('/pages/my/userinfo')"></image>
					<!-- <view class="lovip" v-if="isVip == true&& XCXIsSelect != '否'&&!isLogin">
					<image src="../../static/images/my/vip.png" style="width: 100%;height: 100%;"></image>
				</view> -->
				</view>
				<view class="u-flex-1 u-m-l-10 text-white" v-if="!isLogin">
					<view class="u-font-18  text-bold">
						<view class="flex align-center ">
							<view class=" ">{{userName}}</view>
						</view>
						<view class=" text-sm">ID:{{invitationCode}}</view>
					</view>

					<!-- 	<view class="u-font-14 u-tips-color" style="color: #000000;font-size: 24rpx;margin-top: 5rpx;"
					@click="goNav('/pages/me/vip/index')">邀请码:{{invitationCode}}</view> -->
				</view>
				<view v-else class="text-xl u-p-l-20 text-bold" @click="goLogin('/pages/public/login')">
					登录
				</view>
				<view v-if="!isLogin&&XCXIsSelect != '否'" class="" @click="goNav('/pages/msg/message')">
					<image src="../../static/images/my/msg.png" style="width: 50upx;height: 52rpx;" mode=""></image>
				</view>
			</view>
			<view class=" padding-lr" style="position: relative;" v-if="XCXIsSelect != '否'&&!isLogin">
				<image src="../../static/images/my/bg.png" style="width: 100%;height: 110rpx;" mode=""></image>
				<view class="flex justify-between  margin-lr padding-tb-sm radius"
					style="position: absolute;top: 0;left:550upx;">
					<view v-if="!isVip" class="btn-bg" style="color: #313237;" @click="goNav('/my/vip/index')">去开通
					</view>
					<view v-if="isVip" class="btn-bg" style="color: #313237;" @click="goNav('/my/vip/index')">已开通</view>
				</view>
			</view>

			<view class="flex align-center justify-between margin" v-if="XCXIsSelect != '否'">
				<view class="box" @click="goNavs('/pages/order/index')">
					<image src="../../static/images/my/order.png" style="width: 60upx;height: 60upx;"></image>
					<view class="margin-left-sm">
						<view class="text-lg text-bold" style="color: #333333;">我的订单</view>
						<view class="text-sm" style="color: #999999;">查看全部订单</view>
					</view>
				</view>
				<view class="box" @click="goNav('/my/wallet/index')">
					<image src="../../static/images/my/qianbao.png" style="width: 60upx;height: 60upx;"></image>
					<view class="margin-left-sm">
						<view class="text-lg text-bold" style="color: #333333;">我的钱包</view>
						<view class="text-sm" style="color: #999999;">查看钱包余额</view>
					</view>
				</view>
			</view>

			<view class="margin-lr flex align-center justify-between" v-if="XCXIsSelect != '否'">
				<view class="flex align-center justify-between" style="position: relative;color: #fff;"
					@click="applyhome()">
					<image src="../../static/images/my/daisang.png" style="width: 333upx;height: 160upx;"></image>
					<view style="position: absolute;left:15rpx;">
						<view class="text-lg text-bold">代理商中心</view>
						<view class="text-sm margin-top-xs">申请成为代理商</view>
					</view>
				</view>
				<view class="flex align-center justify-between" style="position: relative;color: #fff;"
					@click="tuiguang()">
					<image src="../../static/images/my/tuiguang.png" style="width: 333upx;height: 160upx;"></image>
					<view style="position: absolute;left:15rpx;">
						<view class="text-lg text-bold">推广中心</view>
						<view class="text-sm margin-top-xs">进入立即推广</view>
					</view>
				</view>
			</view>

			<view class="tanbox padding-tb">
				<view class="text-lg text-bold padding-lr">推荐工具</view>
				<view class="flex flex-wrap margin-top">
					<view class="image_tu" @click="goNav('/my/zhandian/myLaundry')" v-if="laundrySel">
						<image src="../../static/images/my/zhandian.png" style="width: 58upx;height: 58upx;"></image>
						<view>站点管理</view>
					</view>
					<view class="image_tu" @click="goNav('/my/order/tousuList')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/tousu.png" style="width: 58upx;height: 58upx;"></image>
						<view>投诉记录</view>
					</view>
					<view class="image_tu" @click="goNav('/my/other/car?type=1')">
						<image src="../../static/images/my/che.png" style="width: 58upx;height: 58upx;"></image>
						<view>我的车库</view>
					</view>
					<view class="image_tu" @click="goNav('/my/other/work')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/zhaopin.png" style="width: 58upx;height: 58upx;"></image>
						<view>职务招聘</view>
					</view>
					<view class="image_tu" @click="goNav('/pages/my/invitationUser')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/yaoqing.png" style="width: 58upx;height: 58upx;"></image>
						<view>分享好友</view>
					</view>
					<view class="image_tu" @click="goNav('/my/other/cooperation')"
						v-if="XCXIsSelect != '否'&&isAgent!=1">
						<image src="../../static/images/my/jiameng.png" style="width: 58upx;height: 58upx;"></image>
						<view>招商加盟</view>
					</view>
					<view class="image_tu" @click="goNav('/my/youhuijuan/index')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/lqzx.png" style="width: 58upx;height: 58upx;"></image>
						<view>领券中心</view>
					</view>
					<view class="image_tu" @click="goNav('/my/youhuijuan/youhui')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/wdqb.png" style="width: 58upx;height: 58upx;"></image>
						<view>我的券包</view>
					</view>
					<view class="image_tu" @click="goNav('/pages/youhuika/youhui')" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/yhk.png" style="width: 58upx;height: 58upx;"></image>
						<view>我的优惠卡</view>
					</view>
					<view class="image_tu" @click="Changekefu()" v-if="XCXIsSelect != '否'">
						<image src="../../static/images/my/kefu.png" style="width: 58rpx;height: 58upx;"></image>
						<view>客服中心</view>
					</view>
					<!-- 	<view class="image_tu" @click="goNav('/pages/msg/message')" v-if="XCXIsSelect != '否'">
					<image src="../../static/images/my/msg.png" style="width: 58upx;height: 58upx;"></image>
					<view>系统消息</view>
				</view> -->
					<view class="image_tu" @click="goNav('/my/feedbackIndex/feedbackIndex') ">
						<image src="../../static/images/my/help.png" style="width: 58upx;height: 58upx;"></image>
						<view>帮助中心</view>
					</view>
					<view class="image_tu" @click="goNav('/my/setting/index')">
						<image src="../../static/images/my/set.png" style="width: 58upx;height: 58upx;"></image>
						<view>设置中心</view>
					</view>
				</view>
			</view>
			<image v-if="kaImg" @click="goNav(kaUrl)" :src="kaImg"
				style="width: 696rpx;height: 240rpx;margin-left: 30rpx;border-radius: 24rpx;"></image>

		</view>
</template>

<script>
	export default {
		data() {
			return {
				avatar: '../../static/logo.png',
				isLogin: true,
				userName: '匿名',
				browse: 0, //浏览数
				fans: 0, //粉丝数
				follow: 0, //关注数
				visitor: 0, //访客数
				userId: '',
				isVip: false,
				laundrySel: false,
				invitationCode: '', //邀请码
				kaImg: '',
				kaUrl: '',
				geRen: 0,
				Qe: 0,
				XCXIsSelect: '否',
				renzheng: false,
				isPromotion: '', //推广员  1是  0或者null 不是
				isAgent: '', //代理商  1是  0或者null 不是
			}
		},
		onLoad() {
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");

		},
		onShow() {
			this.Qe = uni.getStorageSync("Qe")
			this.geRen = uni.getStorageSync("geRen")
			if (this.Qe == 2 || this.geRen == 2) {
				this.renzheng = false
			} else {
				this.renzheng = true
			}
			this.getBottomImg();
			this.userId = uni.getStorageSync('userId')
			if (this.userId) {
				this.isLogin = false
				this.getZZSel();
				this.getUserInfo()
				this.getRenZheng()
				this.getRenZhengs()
				this.getAmount()
				this.getIsVip()
			} else {
				this.laundrySel = false
				this.isLogin = true
				this.userName = '匿名'
				this.browse = 0
				this.fans = 0
				this.follow = 0
				this.visitor = 0
				this.avatar = '../../static/logo.png'
			}

		},
		methods: {
			getBottomImg() {
				this.$Request.getT('/app/banner/selectBannerList?classify=7').then(res => {
					if (res.code == 0 && res.data) {
						if (res.data.length > 0) {
							this.kaImg = res.data[0].imageUrl;
							this.kaUrl = res.data[0].url;
						}
					}
				});
			},
			getZZSel() {
				this.$Request.getT('/app/laundry/getMyLaundryList?userId=' + this.userId + '&page=0&limit=10&laundryName=').then(
				res => {
					if (res.status == 0) {
						if (res.data && res.data.content && res.data.content.length > 0) {
							this.laundrySel = true;
						} else {
							this.laundrySel = false;
						}
					} else {
						this.laundrySel = false;
					}
				});
			},
			applyhome() {
				if (this.userId) {
					if (this.isAgent == 0) {
						uni.navigateTo({
							url: '/my/other/cooperation'
						})
					} else {
						uni.navigateTo({
							url: '/my/other/agent',
						})
					}
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}

			},
			tuiguang() {
				if (this.userId) {
					if (this.isPromotion == 0) {
						uni.navigateTo({
							url: '/my/other/team'
						})
					} else {
						uni.navigateTo({
							url: '/my/team/team',
						})
					}
				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}

			},
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
			goNav(e, name) {
				console.log(e)
				if (this.userId) {
					uni.navigateTo({
						url: e
					})

				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			goNavs(e, name) {
				console.log(e)
				if (this.userId) {
					uni.switchTab({
						url: e
					})

				} else {
					uni.showModal({
						title: '提示',
						content: '您还未登录,请先登录',
						success: function(res) {
							if (res.confirm) {
								console.log('用户点击确定');
								uni.navigateTo({
									url: '/pages/public/login'
								})
							} else if (res.cancel) {
								console.log('用户点击取消');
							}
						}
					})
				}
			},
			goLogin(e) {
				uni.navigateTo({
					url: e
				})
			},
			getAmount() {
				this.$Request.get("/app/userBrowse/selectAmount").then(res => {
					if (res.code == 0) {
						this.browse = res.data.browse
						this.fans = res.data.fans
						this.follow = res.data.follow
						this.visitor = res.data.visitor
					}
				});
			},
			getUserInfo() {
				this.$Request.get("/app/user/selectUserById").then(res => {
					if (res.code == 0) {
						this.userName = res.data.userName
						this.invitationCode = res.data.invitationCode
						this.avatar = res.data.avatar ? res.data.avatar : '../../static/logo.png'
						this.isAuthentication = res.data.isAuthentication

						if (res.data.isPromotion == null || res.data.isPromotion == 0) { //推广员
							this.isPromotion = '0'
						} else {
							this.isPromotion = res.data.isPromotion
						}
						if (res.data.isAgent == null || res.data.isAgent == 0) { //代理商
							this.isAgent = '0'
						} else {
							this.isAgent = res.data.isAgent
						}

						uni.setStorageSync('feiRate', res.data.feiRate) //代理商佣金比例
						uni.setStorageSync('isAuthentication', res.data.isAuthentication)
						uni.setStorageSync('avatar', res.data.avatar)
						uni.setStorageSync('invitationCode', res.data.invitationCode)
						uni.setStorageSync('zhiFuBao', res.data.zhiFuBao)
						uni.setStorageSync('zhiFuBaoName', res.data.zhiFuBaoName)

					}
				});
			},
			// 个人认证数据
			getRenZheng() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 1).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) {
							this.geRen = 0
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 0) {
							this.geRen = 1
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 1) {
							this.geRen = 2
							uni.setStorageSync("geRen", this.geRen)
						} else if (res.data.status == 2) {
							this.geRen = 3
							uni.setStorageSync("geRen", this.geRen)
						}
					}
				});
			},
			// 企业认证数据
			getRenZhengs() {
				let classify
				this.$Request.get("/app/userCertification/queryInsert?classify=" + 2).then(res => {
					console.log(res)
					if (res.code == 0) {
						if (res.data == null) { //未实名
							this.Qe = 0
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 0) { //审核中
							this.Qe = 1
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 1) { //已实名
							this.Qe = 2
							uni.setStorageSync("Qe", this.Qe)
						} else if (res.data.status == 2) { //已拒绝
							this.Qe = 3
							uni.setStorageSync("Qe", this.Qe)
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
			}

		}
	}
</script>

<style lang="scss">
	page {
		background: #F1F5FE;
	}

	.bg {
		background: #FFFFFF;
	}

	.camera {
		width: 54px;
		height: 44px;

		&:active {
			background-color: #ededed;
		}
	}

	.btn-bg {
		width: 64px;
		height: 28px;
		background: linear-gradient(90deg, #fffbe5 0%, #eddcb8 100%);
		border-radius: 28px;
		text-align: center;
		line-height: 28px;
		margin-top: 4px;
		color: '#604320'
	}

	.images {
		width: 18rpx;
		height: 30rpx;
	}

	.dfs {
		display: flex;
		align-items: center;
	}

	.lovip {
		width: 100rpx;
		height: 35rpx;
		position: relative;
		top: -39rpx;
	}

	.box {
		width: 48%;
		border-radius: 16upx;
		display: flex;
		align-items: center;
		background: #FFFFFF;
		padding: 30upx;
	}

	.tanbox {
		background: #FFFFFF;
		border-radius: 24upx;
		margin: 30rpx 30upx 0rpx;

	}

	.image_tu {
		text-align: center;
		width: 25%;
		margin-bottom: 30upx;
	}
</style>
