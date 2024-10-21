<template>
	<view>
		<!-- #ifdef MP-WEIXIN -->
		<view class="part1">
			<view class="mobile">
				<u-field v-model="mobile" placeholder="联系电话" icon="phone-fill" maxlength="11" type="number" label-align="center">
				</u-field>
			</view>
			<view class="name">
				<u-field v-model="name" placeholder="姓名" icon="account-fill" label-align="center">
				</u-field>
			</view>

			<view class="address" @click="bindmap">
				<u-field v-model="cityaddress" placeholder="地址" :disabled="true" icon="map-fill" label-align="center">
				</u-field>
			</view>
			<view class="detailaddress">
				<u-field v-model="detailaddress" placeholder="详细地址" icon="map-fill" label-align="center">
				</u-field>
			</view>
			<view
				style="height: 100upx;background:#FFFFFF;display: flex;margin-top: 30upx;justify-content: space-between;padding: 0rpx 22rpx;">
				<view style="font-size: 30upx;color: #333333;text-align: left;padding: 30upx;width: 70%;">设为默认地址</view>
				<switch type="switch" :checked='isDefault === 1 ? true : false' @change="switch1Change"
					style="padding: 20upx;transform:scale(0.9)" color="#557EFD" />
			</view>
			<view class="btn" @click="bindhelp">确定</view>
		</view>
		<!-- <view style="margin-top: 20upx;background-color: #FFFFFF;height: max-content;padding: 20upx 20upx 20upx 35upx;">
			<input type="text" placeholder="收货人" style="height: 80upx;color: #000000;font-size: 30upx;" v-model="consignee" />
			<input type="number" placeholder="手机号码" maxlength="11" style="height: 80upx;margin-top: 20upx;color: #000000;font-size: 30upx;"
			 v-model="mobile" />
			<view style="height: 1upx;margin-top: 50upx;"></view>
			<pickerAddress style="padding-bottom: 20upx;color: #000000;font-size: 30upx;height: 30upx;" @change="change">{{provinces}}</pickerAddress>
			<input type="text" v-model="detail" placeholder="详情地址:如道路，门牌号，小区，楼栋号，单元室等" style="height: 80upx;margin-top: 30upx;color: #000000;font-size: 30upx;" />
		</view> -->

		<!-- #endif -->
		<!-- #ifndef MP-WEIXIN -->
		<view class="part1">
			<view class="mobile">
				<u-field v-model="mobile" placeholder="联系电话" icon="phone-fill" maxlength="11" label-align="center">
				</u-field>
			</view>
			<view class="name">
				<u-field v-model="name" placeholder="姓名" icon="account-fill" label-align="center">
				</u-field>
			</view>

			<view class="address" @click="bindmap">
				<u-field v-model="cityaddress" placeholder="地址" :disabled="true" icon="map-fill" label-align="center">
				</u-field>
			</view>
			<view class="detailaddress">
				<u-field v-model="detailaddress" placeholder="详细地址" icon="map-fill" label-align="center">
				</u-field>
			</view>
			<view
				style="height: 100upx;background:#FFFFFF;display: flex;margin-top: 30upx;justify-content: space-between;padding: 0rpx 22rpx;">
				<view style="font-size: 34upx;color: #333333;text-align: left;padding: 30upx;width: 70%;">设为默认地址</view>
				<switch type="switch" :checked='isDefault === 1 ? true : false' @change="switch1Change"
					style="padding: 20upx;transform:scale(0.9)" color="#557EFD" />
			</view>
			<view class="btn" @click="bindhelp">确定</view>
		</view>
		<!-- #endif -->
		<!-- <button
			style="width: 85%;height: 90upx;background: #FF332F;font-size: 32upx;color: #FFFFFF;margin-top: 40upx;border-radius: 50upx;line-height: 100upx;"
			@tap='setEditAddress'>保存</button> -->
	</view>
</template>

<script>
	// import pickerAddress from '@/components/wangding-pickerAddress/wangding-pickerAddress.vue'
	import pickerAddress from '@/my/components/wangding-pickerAddress/wangding-pickerAddress.vue'
	export default {
		components: {
			pickerAddress
		},
		data() {
			return {
				// provinces: '选择地址',
				// consignee: '',
				// mobile: '',
				// detail: '',
				// createAt: '',
				id: '',
				isDefault: 0,
				// id: 0,
				mobile: '',
				name: '',
				cityaddress: '',
				detailaddress: '',
				latitude: '',
				longitude: '',
				province:'',
				city:'',
				district:''
			}
		},
		onLoad(d) {
			let id = d.id;
			if (id) {
				this.id = d.id;
				uni.setNavigationBarTitle({
					title: '编辑地址'
				});
				this.getAddressList(id);
			} else {
				uni.setNavigationBarTitle({
					title: '添加地址'
				});
			}
		},
		methods: {
			change(data) {
				this.provinces = data.data.join('')
			},
			bindmap() {
				var that = this
				// if (that.ciaddress == '') {
				uni.chooseLocation({
					success: function(res) {
						// console.log(res)
						console.log('位置名称：' + res.name);
						console.log('详细地址：' + res.address);
						console.log('纬度：' + res.latitude);
						console.log('经度：' + res.longitude);
						that.detailaddress = res.name
						that.latitude = res.latitude
						that.longitude = res.longitude
						that.shengcheng(res.longitude, res.latitude)
					}
				});
				// }
			},
			shengcheng(longitude, latitude) {
				this.$Request.getT('/app/Login/selectCity?lat=' + latitude + '&lng=' + longitude).then(res => {
					// console.log(res)
					if (res.code === 0) {
						this.cityaddress = res.data.province + res.data.city + res.data.district
						// console.log(this.address)
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
					}
				});

			},
			getAddressList(id) {
				uni.showLoading({
					title: '加载中...'
				});
				this.$Request.getT('/app/address/selectAddressByAddressId?addressId=' + id).then(res => {
					console.log(res)
					if (res.code == 0) {
						this.name = res.data.name;
						this.mobile = res.data.phone;
						this.cityaddress =res.data.province+ res.data.city+res.data.district;
						this.detailaddress = res.data.detailsAddress;
						this.isDefault = res.data.isDefault;
						this.userId = res.data.userId;
						this.latitude = res.data.latitude;
						this.longitude = res.data.longitude;
						this.province = res.data.province
						this.city = res.data.city
						this.district = res.data.district
					}
					uni.hideLoading();
				});
			},
			bindhelp() {
				if (this.id) {
					this.$queue.showLoading('修改中...')
				} else {
					this.$queue.showLoading('添加中...')
				}
				let userId = this.$queue.getData('userId');
				let data = {
					name: this.name,
					phone: this.mobile,
					province:this.province,
					city:this.city,
					district:this.district,
					detailsAddress:this.detailaddress,
					isDefault:this.isDefault,
					addressId: this.id,
					// address: this.cityaddress,
					// addressDetail: this.detailaddress,
					latitude: this.latitude,
					longitude: this.longitude,
					// userId: userId,
					
					// addressDefault: this.isDefault
				}
				if (this.mobile.length < 11) {
					uni.showToast({
						icon: 'none',
						title: '请输入正确的电话信息'
					});
					return;
				}
				if (this.name != undefined && this.name != '') {} else {
					uni.showToast({
						icon: 'none',
						title: '收货人不能为空'
					});
					return;
				}
				if (this.cityaddress == '选择地址') {
					uni.showToast({
						icon: 'none',
						title: '请选择地址信息'
					});
					return;
				}
				if (this.detailaddress != undefined && this.detailaddress != '') {} else {
					uni.showToast({
						icon: 'none',
						title: '地址信息不能为空'
					});
					return;
				}

				if (this.id) {
					this.$Request.postJson('/app/address/updateAddress', data).then(res => {
						if (res.code == 0) {
							uni.hideLoading()
							this.$queue.showToast("修改成功!");
							setTimeout(d => {
								uni.navigateBack();
							}, 1000)
						} else {
							uni.hideLoading()
							this.$queue.showToast(res.msg);
						}
					});
				} else {
					this.$Request.postJson('/app/address/insertAddress', data).then(res => {
						if (res.code == 0) {
							uni.hideLoading()
							this.$queue.showToast("添加成功!");
							setTimeout(d => {
								uni.navigateBack();
							}, 1000)
						} else {
							uni.hideLoading()
							this.$queue.showToast(res.msg);
						}
					});
				}
			},
			//校验手机格式
			checkMobile(mobile) {
				return RegExp(/^1[34578]\d{9}$/).test(mobile);
			},
			switch1Change(e) {
				this.isDefault = e.detail.value == true ? 1 : 0;
			}
		}
	}
</script>

<style>
	body {
		background: #FFFFFF;
	}

	.part1 {
		width: 100%;
		background: #ffffff;
		margin-top: 24rpx;
		padding-bottom: 40upx;
	}

	.btn {
		width: 90%;
		height: 80upx;
		background: #557EFD;
		border-radius: 14upx;
		margin: 0 auto;
		color: white;
		text-align: center;
		line-height: 80upx;
		letter-spacing: 2upx;
		margin-top: 40upx;
	}

	.u-icon__icon {
		font-size: 45rpx !important;
	}
	
	.u-field {
		padding: 35rpx 28rpx !important;
	}
	
	.u-label {
		flex: 0 0 42px !important;
	}
	.u-field__input-wrap {
	    font-size: 30rpx !important;
	}
	.u-textarea-class {
	    font-size: 30rpx !important;
	}
</style>
