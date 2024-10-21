<template>
	<view class="padding-bottom">
		<view class="u-skeleton">
			<view class="u-skeleton-fillet" style="width: 100%;height: 490rpx;">
				<image class="u-skeleton-fillet" style="width: 100%;height: 100%;" :src="order.homepageImg"
					@click="saveImg(order.homepageImg)"></image>
			</view>
			<view class="padding-sm bgs u-skeleton-fillet" style="padding-bottom: 120upx;">
				<view class=" padding flex justify-between bg radius ">
					<!-- <u-avatar :src="order.avatar?order.avatar:'../../../static/logo.png'" mode="square"></u-avatar> -->
					<view class="flex-sub ">
						<view class="text-xxl flex align-center" style="color: #FF2D01;"
							v-if="order.memberMoney||order.money">
							<view class=""><text class="text-df ">￥</text>{{order.money}}
							</view>
							<view style="font-size: 24rpx;color: #999999;margin-left: 10rpx;color: #FF2D01;"
								class="text-df">会员尊享￥<text style="color: #FF2D01;">{{order.memberMoney}}</text></view>
						</view>
						<view class="flex justify-between">
							<view class="text-lg margin-top-xs margin-bottom-sm u-line-1" style="width: 500rpx;"
								v-if="order.myLevel">
								{{order.myLevel}}
							</view>
							<view class="" style="color: #999999;">已服务{{order.count}}人</view>
						</view>
						<view class="margin-top-xs flex align-center">
							<view class="flex align-center margin-right-xs" style="color: #999999;"
								v-for="(item,index) in order.gameName" :key="index">
								{{item}}
								<view style="width: 1rpx;height: 21upx;background: #CCCCCC;margin-left: 10upx;"
									v-if="lines-1!=index"></view>
							</view>
						</view>
					</view>
				</view>

				<view class="margin-top-sm padding bg text-white radius u-skeleton-fillet">
					<view class="flex align-center margin-bottom">
						<view style="color: #999999;">服务项目</view>
						<view class="margin-left">{{order.serviceName}}</view>
					</view>
					<view class="flex align-center margin-bottom">
						<view style="color: #999999;">适合车型</view>
						<view class="margin-left">{{order.carType }}</view>
					</view>
					<view class="flex align-center">
						<view style="color: limegreen;">服务保障</view>
						<view class="margin-left">{{order.safeguard}}</view>
					</view>
				</view>

				<view class="margin-top-sm padding bg text-white radius u-skeleton-fillet">
					<view class="flex align-center justify-between">
						<view class="text-lg">用户评价</view>
						<view class="flex align-center" @tap="goComment()" v-if="commentList.length!=0">
							<view class="margin-right-xs text-sm">查看全部</view>
							<image src="../../../static/images/my/right.png" style="width: 12upx;height: 20upx;">
							</image>
						</view>
					</view>
					<view v-if="commentList.length<=0" class="margin-top-sm"> 暂无评价</view>
					<view class="margin-tb-sm" v-else>
						<view class="flex justify-between">
							<u-avatar :src="commentList[0].avatar" size="48"></u-avatar>
							<view class="flex-sub margin-left-sm" style="line-height: 46upx;">
								{{commentList[0].userName}}
							</view>
							<view class="flex">
								<u-icon v-for="ite in commentList[0].score" :key='ite' color="#2087fe" name="star-fill">
								</u-icon>
							</view>
						</view>
						<view class="margin-top-sm">{{commentList[0].content}}</view>
					</view>

				</view>
				<view class="margin-top-sm bg radius padding-bottom">

					<view class="flex text-center text-white text-lg bg">
						<view class="title_btn flex-sub bg" @click="cut(1)" :class="title_color==1?'bgCol2': ''">服务详情
						</view>
						<view class="title_btn flex-sub bg" @click="cut(2)" :class="title_color==2?'bgCol2': ''">温馨提示
						</view>
						<view class="title_btn flex-sub bg" @click="cut(3)" :class="title_color==3?'bgCol2': ''">服务流程
						</view>
					</view>
					<view class="" v-if="title_color == 1">
						<u-parse class='margin-top' style="padding: 0rpx 20rpx;" :html="order.detailsImg"></u-parse>
					</view>

					<view class="" v-if="title_color == 2">
						<u-parse class='margin-top' style="padding: 0rpx 20rpx;" :html="wxtsMsg"></u-parse>
					</view>

					<view class="" v-if="title_color == 3">
						<u-parse class='margin-top' style="padding: 0rpx 20rpx;" :html="fwlcMsg"></u-parse>
					</view>

				</view>

			</view>

			<view class="foot" v-if="myId != order.userId">
				<!-- <view @click="follow" class="padding-lr" style="width: 120rpx;">
				<image src="../../../static/images/index/guanzhu.png" v-if="!isFollow"
					style="width: 49rpx;height: 43rpx;"></image>
				<image src="../../../static/images/index/guanzhu1.png" v-else style="width: 49rpx;height: 43rpx;">
				</image>
				<view style="font-size: 22upx;">关注</view>
			</view> -->
				<!-- <view @click="goMsg" class="padding-lr" style="width: 120rpx;">
					<image src="../../../static/images/index/im.png" style="width: 49rpx;height: 43rpx;"></image>
					<view style="font-size: 22upx;">聊天</view>
				</view> -->
				<view v-if="XCXIsSelect != '否'" style="padding: 10rpx 0rpx;">
					<view class="text-bold" style="font-size: 52upx;color: #FF2D01;"
						v-if="order.memberMoney ||order.money">
						<text style="font-size: 38upx;">￥</text>{{isVip? order.memberMoney :order.money}}
					</view>
					<view class="tetx-sm" style="color: #999999;" v-if="order.serviceName">{{order.serviceName}}</view>
				</view>
				<view class="buttons" @click="goNav()">立即下单</view>
			</view>
		</view>

		<u-skeleton :loading="loading" :animation="true" elColor='#FFFFFF' bgColor='#FFFFFF'></u-skeleton>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				wxtsMsg: '',
				fwlcMsg: '',
				title_color: 1,
				game: [],
				loading: true, // 是否显示骨架屏组件
				id: '',
				orderDet: {},
				page: 1,
				limit: 10,
				order: {},
				commentList: [],
				isFollow: false,
				myId: '',
				isVip: false,
				AUDIO: uni.createInnerAudioContext(),
				isPlay: false,

				mobile: '',
				name: '',
				cityaddress: '',
				detailaddress: '',
				latitude: '',
				longitude: '',
				province: '',
				city: '',
				district: '',
				lines: '',
				XCXIsSelect: '否',
			}
		},
		onShareAppMessage(res) {
			return {
				path: '?invitation=' + this.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.order.myLevel,
				imageUrl: this.order.homepageImg
			}
		},
		/*
		 * uniapp微信小程序分享页面到微信朋友圈
		 */
		onShareTimeline(res) {
			return {
				path: '/pages/index/game/order?invitation=' + this
					.invitationCode, //这是为了传参   onload(data){let id=data.id;} 
				title: this.order.myLevel,
				imageUrl: this.order.homepageImg
			}
		},
		onLoad(option) {
			this.id = option.id
			if (this.id) {
				this.getDet()
				this.getOrderComment()
			}

			this.myId = uni.getStorageSync('userId')
			this.isVip = uni.getStorageSync('isVIP')
			this.XCXIsSelect = this.$queue.getData("XCXIsSelect");
			console.log(this.isVip)

		},
		onShow() {
			this.wxtsMsg = this.$queue.getData('wxtsMsg');
			this.fwlcMsg = this.$queue.getData('fwlcMsg');
		},
		onReady() {
			this.AUDIO.onEnded(function(res) {
				this.isPlay = false;
			});
		},
		methods: {
			cut(index) {
				this.title_color = index;
			},
			goComment() {
				console.log('pppppp')
				uni.navigateTo({
					url: '/my/other/commentList?id=' + this.order.id
				})
			},
			//详情图片放大
			saveImg(imgs, index) {
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
			// 地址
			bindAdd() {
				uni.navigateTo({
					url: '../../../my/address/address'
				})
			},
			// 一键导航
			bindGps(latitude, longitude, name) {
				uni.openLocation({
					latitude: latitude - 0, //要去的纬度-地址       
					longitude: longitude - 0, //要去的经度-地址
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
			// 详情
			getDet() {
				uni.showLoading({
					title: '加载中...',
					// icon: 'none'
				});
				this.$Request.get("/app/orderTaking/queryTakingDetails", {
					id: this.id,
					latitude: uni.getStorageSync('latitude'),
					longitude: uni.getStorageSync('longitude')
				}).then(res => {
					if (res.code == 0) {
						this.order = res.data
						if (this.order.homepageImg) {
							this.order.homepageImg = this.order.homepageImg.split(",");
						}
						if (this.order.gameName) {
							this.order.gameName = this.order.gameName.split(",");
						}
						// if (this.order.detailsImg) {
						// 	this.order.detailsImg = this.order.detailsImg.split(",");
						// }
						this.lines = this.order.gameName.length

						this.selectFollow()

					}
					this.loading = false;
					uni.hideLoading();

				});

			},
			// 评论
			getOrderComment() {
				this.$Request.get("/app/takingComment/selectOrderTakingComment", {
					id: this.id,
					page: this.page,
					limit: this.limit
				}).then(res => {
					if (res.code == 0) {
						this.commentList = [...this.commentList, ...res.data.list]
					}
				});
			},
			// 是否关注
			selectFollow() {
				this.$Request.get("/app/userFollow/selectFollowUser", {
					followUserId: this.order.userId
				}).then(res => {
					if (res.data == true) {
						this.isFollow = true
					} else {
						this.isFollow = false
					}

				});
			},
			playVoice() {
				console.log(this.isPlay)
				this.AUDIO.src = this.order.voiceIntroduce;
				if (this.isPlay) {
					this.AUDIO.stop();
				} else {
					this.AUDIO.play();
				}
				this.isPlay = !this.isPlay;
			},
			goNav() {
				// if (this.startTime == '') {
				// 	uni.showToast({
				// 		icon: 'none',
				// 		title: '请选择上门时间'
				// 	});
				// 	return;
				// }
				uni.navigateTo({
					url: "/pages/index/game/orderDet?id=" + this.id
				})
			},
			goMsg() {
				let data = {
					userId: this.myId,
					focusedUserId: this.order.userId
				}
				this.$Request.postJson('/app/chat/insertChatConversation ', data).then(res => {
					if (res.code == 0) {
						let id = this.order.userId == res.data.userId ? res.data.focusedUserId : this.order.userId
						uni.navigateTo({
							url: '/pages/msg/im?chatConversationId=' + res.data.chatConversationId +
								'&byUserId=' + id
						})
					}
				})
			},
			// 关注
			follow() {
				let that = this
				that.$Request.get("/app/userFollow/insert", {
					followUserId: that.order.userId
				}).then(res => {
					uni.showToast({
						title: res.msg,
						icon: 'none'
					})
					setTimeout(function() {
						that.selectFollow()
					}, 500)
				});
			}
		}

	}
</script>

<style>
	page {
		background-color: #FFFFFF;
	}

	.bgCol2 {
		color: #557EFD;
	}

	.title_btn {
		height: 78upx;
		line-height: 78upx;
		/* background: #f7f7f7; */
	}

	.bgs {
		background-color: #F7F7F7;
	}

	.bg {
		background-color: #FFFFFF;
	}

	.line_s {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #1AD566;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.line_x {
		display: inline-flex;
		width: 14rpx;
		height: 14rpx;
		background: #000000;
		border-radius: 50%;
		margin-right: 10rpx;
	}

	.u-size-default {
		background: #557EFD !important;
	}

	.foot {
		position: fixed;
		bottom: 0;
		left: 0;
		right: 0;
		z-index: 999;
		background: #FFFFFF;
		padding: 0upx 0upx 0upx 30upx;
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.buttons {
		background: #0175FE;
		/* border-radius: 44upx; */
		color: #FFFFFF;
		display: inline-block;
		/* padding: 23upx 60upx; */
		width: 260rpx;
		height: 110rpx;
		font-size: 28upx;
		text-align: center;
		line-height: 110rpx;
	}
</style>
