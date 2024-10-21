<!--
 * @Author: dfh
 * @Date: 2022-04-20 09:43:44
 * @LastEditors: dfh
 * @Modified By: dfh
 * @describe: 
-->
<template>
	<view class='dialog-license-container' @click='cancelHandler'>
		<view class="dialog-license-box"  @click.stop="">
			<text class="dialog-title">添加车牌</text>
			<text class="dialog-sub-title">请输入您的车牌号</text>
			<view class="dialog-row-license-box">
				<template v-for="(item,index) in brand">
					<text v-if="item=='能源'" :key='index' class="dialog-license new-energy" @click.stop="onBrandClick"
						:data-index='index'>{{item}}</text>
					<text v-else :key='index' class="dialog-license" :class="{'dialog-active': currentIndex===index}"
						@click.stop="onBrandClick" :data-index='index'>{{item}}</text>
				</template>
			</view>
			<button type="primary" class="dialog-add-license" @click.stop="submitHandler">确认</button>
		</view>

		<!-- 省份 -->
		<view class="dialog-province-box" v-if="isShowProvinceSelect"  @click.stop="">
			<view class="dialog-header">
				<text class="dialog-cancel" @click.stop='cancelHandler'> 取消</text>
				<text @click.stop="submitHandler">完成</text>
			</view>
			<view class="dialog-grid-box">
				<text v-for="(item,index) in province" :key='index' @click.stop="provinceClick"
					:data-item="item">{{item}}</text>
				<view class="dialog-close-box" @click.stop="clearHandler">
					<image src="../../static/add_car_delete.png" mode="widthFix" style="width:60rpx;height:60rpx" />
				</view>
			</view>
		</view>

		<!-- 车牌 -->
		<view class="dialog-province-box" v-if="isShowBrandSelect" @click.stop="">
			<view class="dialog-header">
				<text class="dialog-cancel" @click.stop='cancelHandler'>取消</text>
				<text @click.stop="submitHandler">完成</text>
			</view>
			<view class="dialog-grid-box">
				<text v-for="(item,index) in brand_code" :key='index' @click.stop="brandCodeHandler" :data-item="item"
					:class="{'dialog-gray-text':!item.isShow}">{{item.code}}</text>
				<text @click.stop="clearHandler" class="dialog-close">删除</text>
			</view>
		</view>
	</view>


</template>

<script>
	export default {
		props: {
			onCancel: {
				type: Function
			},
			onOk: {
				type: Function
			}
		},
		data() {
			return {
				isEnd: false, //是否可以结束输入
				isNotEnergy: true,
				brand: ['', '', '', '', '', '', '', '能源'],
				currentIndex: 0,
				province: ['京', '津', '沪', '渝', '冀', '豫', '云', '辽', '黑', '湘', '皖', '鲁', '新', '苏', '浙', '赣', '鄂', '桂', '甘',
					'晋', '蒙', '陕', '吉', '闽', '贵', '粤', '青', '藏', '川', '宁', '琼', '使', '无'
				],
				brand_code: [{
						code: '1',
						isShow: true
					},
					{
						code: '2',
						isShow: true
					},
					{
						code: '3',
						isShow: true
					},
					{
						code: '4',
						isShow: true
					},
					{
						code: '5',
						isShow: true
					},
					{
						code: '6',
						isShow: true
					},
					{
						code: '7',
						isShow: true
					},
					{
						code: '8',
						isShow: true
					},
					{
						code: '9',
						isShow: true
					},
					{
						code: '0',
						isShow: true
					},
					{
						code: 'Q',
						isShow: true
					},
					{
						code: 'W',
						isShow: true
					},
					{
						code: 'E',
						isShow: true
					},
					{
						code: 'R',
						isShow: true
					},
					{
						code: 'T',
						isShow: true
					},
					{
						code: 'Y',
						isShow: true
					},
					{
						code: 'U',
						isShow: true
					},
					{
						code: 'O',
						isShow: false
					},
					{
						code: 'P',
						isShow: true
					},
					{
						code: '港',
						isShow: false
					},
					{
						code: 'A',
						isShow: true
					},
					{
						code: 'S',
						isShow: true
					},
					{
						code: 'D',
						isShow: true
					},
					{
						code: 'F',
						isShow: true
					},
					{
						code: 'G',
						isShow: true
					},
					{
						code: 'H',
						isShow: true
					},
					{
						code: 'J',
						isShow: true
					},
					{
						code: 'K',
						isShow: true
					},
					{
						code: 'L',
						isShow: true
					},
					{
						code: '澳',
						isShow: false
					},
					{
						code: 'Z',
						isShow: true
					},
					{
						code: 'X',
						isShow: true
					},
					{
						code: 'C',
						isShow: true
					},
					{
						code: 'V',
						isShow: true
					},
					{
						code: 'B',
						isShow: true
					},
					{
						code: 'N',
						isShow: true
					},
					{
						code: 'M',
						isShow: true
					},
					{
						code: '学',
						isShow: false
					},
					{
						code: '领',
						isShow: false
					}
				],
			}
		},
		computed: {
			isShowProvinceSelect() {
				return this.currentIndex === 0
			},
			isShowBrandSelect() {
				return this.currentIndex > 0;
			}
		},
		created() {},
		methods: {
			// 校验车牌
			isVehicleNumber(vehicleNumber){
				let result = false;
				const express =
					/^(([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领][A-Z](([0-9]{5}[DF])|([DF]([A-HJ-NP-Z0-9])[0-9]{4})))|([京津沪渝冀豫云辽黑湘皖鲁新苏浙赣鄂桂甘晋蒙陕吉闽贵粤青藏川宁琼使领][A-Z][A-HJ-NP-Z0-9]{4}[A-HJ-NP-Z0-9挂学警港澳使领]))$/;
				result = express.test(vehicleNumber);
				return result;
			},
			onBrandClick(e) {
				console.log(e.currentTarget.dataset.index);
				this.currentIndex = e.currentTarget.dataset.index;
			},
			provinceClick(e) {
				console.log(e.currentTarget.dataset.item);
				let {
					brand,
					currentIndex
				} = this;
				brand[currentIndex++] = e.currentTarget.dataset.item;
				this.brand = brand;
				this.currentIndex = currentIndex === brand.length ? currentIndex - 1 : currentIndex;
				this.resetKeyboard();
				this.brandIsEnd();
			},
			brandCodeHandler(e) {
				console.log(e.currentTarget.dataset.item);
				if (!e.currentTarget.dataset.item.isShow) return;
				const item = e.currentTarget.dataset.item;
				let {
					brand_code,
					currentIndex,
					brand
				} = this;
				if (currentIndex === 6 && item.code === '澳' || item.code === '港' || item.code === '领' || item.code ===
					'学') { //处理选择了 港澳领学
					brand[currentIndex] = item.code;
					this.isNotEnergy = false; //设置最后一个味是否为灰色
				} else {
					brand[currentIndex++] = item.code;
					this.isNotEnergy = true; //
				}
				this.brand = brand;
				this.brand_code = [...brand_code];
				this.currentIndex = currentIndex === brand.length ? currentIndex - 1 : currentIndex;
				this.resetKeyboard();
				this.brandIsEnd();

			},
			//判断时候结束
			brandIsEnd() {
				let {
					brand
				} = this;
				this.isEnd = !brand.includes('');
			},
			//重新设置键盘
			resetKeyboard() {
				let {
					brand_code,
					currentIndex
				} = this;
				if (currentIndex === 1) {
					brand_code = brand_code.map(item => {
						if (item.code === '澳' || item.code === '港' || item.code === '领' || item.code === '学') {
							console.log('currentIndex', currentIndex)
							return {
								...item,
								isShow: false
							}
						} else {
							return {
								...item,
								isShow: true
							}
						}
					})
				} else if (currentIndex === 6) {
					brand_code = brand_code.map(item => {
						if (item.code === 'O') {
							console.log('currentIndex', currentIndex)
							return {
								...item,
								isShow: false
							}
						} else {
							return {
								...item,
								isShow: true
							}
						}
					})
				} else {
					brand_code = brand_code.map(item => {
						if (item.code === '澳' || item.code === '港' || item.code === '领' || item.code === '学' ||
							item.code === 'O') {
							return {
								...item,
								isShow: false
							}
						} else {
							return {
								...item
							}
						}
					})
				}
				this.brand_code = brand_code;
			},
			clearHandler() {
				let {
					brand,
					currentIndex
				} = this;
				if (currentIndex <= 0) return;
				console.log(currentIndex)
				brand[currentIndex--] = currentIndex === 6 ? '能源' : '';
				this.brand = brand;
				this.currentIndex = currentIndex;
				this.resetKeyboard();
				this.brandIsEnd();
			},
			submitHandler() {
				const brand = [...this.brand];
				if (brand[brand.length - 1] === '能源') {
					brand.pop();
				}
				console.log(brand)
				const licensePlate = brand.join('');
				const checked = this.isVehicleNumber(licensePlate);
				if (checked) {
					this.$emit('onOk', licensePlate);
					this.cancelHandler();
				} else {
					console.log('请输入正确的车牌号码');
					uni.showToast({
						title: '请输入正确的车牌号码',
						icon: 'none'
					})
				}
			},
			cancelHandler() {
				this.isEnd = false; //是否可以结束输入
				this.isNotEnergy = true;
				this.brand = ['', '', '', '', '', '', '', '能源'];
				this.currentIndex = 0;
				this.$emit('onCancel');
			}
		}

	}
</script>

<style lang="less">
	page {
		width: 100%;
		height: 100%;

		.dialog-license-container {
			position: fixed;
			left: 0;
			top: 0;
			right: 0;
			bottom: 0;
			z-index: 999 !important;
			background-color: rgba(0, 0, 0, .3);
			display: flex;
			flex-direction: column;
			align-items: center;

			.dialog-license-box {
				width: 650rpx;
				height: 487rpx;
				background: #FFFFFF;
				border-radius: 35rpx;
				margin-top: 170rpx;
				display: flex;
				flex-direction: column;
				align-items: center;

				.dialog-title {
					margin-top: 50rpx;
					font-size: 35rpx;
					font-weight: 500;
					color: #323233;
					line-height: 49rpx;
				}

				.dialog-sub-title {
					margin-top: 11rpx;
					font-size: 24rpx;
					font-weight: 400;
					color: #979899;
					line-height: 33rpx;
				}

				.dialog-row-license-box {
					margin-top: 65rpx;
					height: 80rpx;
					display: flex;
					align-items: center;
					justify-content: space-between;
					// padding: 0 10rpx;

					.dialog-license {
						width: 64rpx;
						height: 78rpx;
						border-radius: 16rpx;
						border: 2rpx solid #DEDEDE;
						line-height: 84rpx;
						text-align: center;
						box-sizing: border-box;
						font-size: 38rpx;
						color: black;
						line-height: 78rpx;
						margin: 0 6rpx;

						&.dialog-active {
							border: 2rpx solid #80ADEB;
							color: #007AFF;
						}

						&.new-energy {
							background: rgba(18, 191, 145, 0.04);
							border: 2rpx solid rgba(23, 186, 142, 0.46);
							font-size: 18rpx;
							color: #12BF91;
							border: 2rpx solid #80ADEB;
						}
					}

				}

				.dialog-add-license {
					margin-top: 65rpx;
					width: 400rpx;
					height: 78rpx;
					line-height: 78rpx;
				}
			}

			.dialog-province-box {
				width: 100%;
				height: 460rpx;
				position: absolute;
				bottom: 0;
				left: 0;
				border-top: 1rpx solid #e7e8ea;
				display: flex;
				flex-direction: column;
				box-sizing: border-box;
				padding-bottom: 30rpx;
				background: #e7e8ea;

				.dialog-header {
					width: 100%;
					height: 60rpx;
					background: white;
					box-sizing: border-box;
					padding: 0 30rpx;
					display: flex;
					justify-content: space-between;
					line-height: 60rpx;
					font-size: 32rpx;
					color: #0F5BFF;

					.dialog-cancel {
						color: gray;
					}
				}

				.dialog-grid-box {
					flex: 1;
					box-sizing: border-box;
					display: flex;
					flex-wrap: wrap;
					padding-right: 1%;
					align-items: center;
					margin-top: 20rpx;

					>text {
						width: 9%;
						margin-left: 1%;
						background: white;
						text-align: center;
						height: 70rpx;
						line-height: 70rpx;
						border-radius: 8rpx;
					}

					.dialog-gray-text {
						color: #e7e8ea;
					}

					.dialog-close-box {
						position: absolute;
						bottom: 38rpx;
						right: 10rpx;
						width: 80rpx;
						height: 70rpx;
						background: #cfd0d4;
						border-radius: 8rpx;
						display: flex;
						align-items: center;
						justify-content: center;
					}

					.dialog-close {
						background: #cfd0d4;
						font-size: 24rpx;
					}
				}
			}

		}
	}
</style>
