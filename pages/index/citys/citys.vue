<template>
	<view>
		<city-select @cityClick="cityClick" :formatName="formatName" :obtainCitys="obtainCitys" :isSearch="true"
			ref="citys"></city-select>
	</view>
</template>

<script>
	import citys from './citys.js'
	// console.log(citys.length)
	import citySelect from '@/components/city-select/city-select.vue'
	export default {
		data() {
			return {
				//需要构建索引参数的名称（注意：传递的对象里面必须要有这个名称的参数）
				formatName: 'title',
				//当前城市
				activeCity: {},
				//热门城市
				hotCity: [],
				//显示的城市数据
				obtainCitys: [],
			}
		},
		components: {
			citySelect
		},
		onLoad(e) {

			//动态更新数据
			setTimeout(() => {
				//修改数据格式
				this.formatName = 'cityName'
				//修改当前城市
				// this.activeCity = {
				// 	cityName: '南京',
				// 	cityCode: 110100
				// }
				//修改热门城市
				// this.hotCity = [
				// 	{
				// 		cityName: '南京',
				// 		cityCode: 110100
				// 	},
				// 	{
				// 		cityName: '北京',
				// 		cityCode: 110102
				// 	}
				// ]
				//修改构建索引数据
				this.obtainCitys = citys

			}, 100)
		},
		methods: {
			cityClick(item) {

				uni.showToast({
					icon: 'none',
					title: '修改成功',
					mask: true
				})
				uni.setStorageSync('city', item.cityName)
				setTimeout(function() {
					// uni.navigateBack()
					let pages = getCurrentPages(); // 当前页面
					let beforePage = pages[pages.length - 2]; // 上一页
					console.log(beforePage)
					uni.navigateBack({
						success: function() {
							beforePage.onShow(); // 执行上一页的onShow方法
						}
					});
				}, 1000)
			}
		}
	}
</script>

<style></style>
