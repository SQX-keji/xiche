# cowain-add-license

> 添加车牌功能

![](https://tva1.sinaimg.cn/large/e6c9d24egy1h1veq3wxizg20j60xcwp1.gif)

## 使用

```vue
<template>
	<view class="content">
		<button type="default" @click="addLicenseDialog">添加车牌</button>
		<cowain-add-licensee v-if="showLicenseDialog" @onCancel='cancelLicenseDialog' @onOk='okLicense'/>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				showLicenseDialog: false, //是否显示添加车牌的dialog
			}
		},
		methods: {
			//显示添加车牌
			addLicenseDialog() {
				this.showLicenseDialog = true;
			},
			//取消添加车牌
			cancelLicenseDialog() {
				this.showLicenseDialog = false;
			},
			//添加车牌成功
			okLicense(license) {
				console.log(license);
			},
		}
	}
</script>

<style lang="less">
	
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}
</style>
```