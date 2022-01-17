<template>
	<view class="content">
		<web-view src="https://h5.tecprove.com" @message="getH5Message"></web-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				title: '文博数字藏品元宇宙',
				shareTitle:'',// 分享标题
				sharePath:'',// 分享路径
				shareImg:'',// 分享图片
				shareType:'',// 分享类型
			}
		},
		onLoad() {
			wx.showShareMenu({
			  withShareTicket: true,
			  menus: ['shareAppMessage', 'shareTimeline']
			})
		},
		methods: {
			// 获取H5返回的参数
			getH5Message(e){
				// console.log("==========getH5Message=========")
				// console.log(e)
				const h5Info = e.detail.data[e.detail.data.length-1];
				const jsonObj  = JSON.parse(h5Info) || {};
				this.shareType = jsonObj.shareType;
				this.shareTitle = jsonObj.shareTitle;
				this.sharePath = jsonObj.sharePath;
				this.shareImg = jsonObj.shareImg;
			}
		},
		onShareAppMessage(res) {
			// console.log("==========onShareAppMessage=========")
			// console.log(res)
			if (this.shareType == 'CUST') {
				return {
					title: this.shareTitle,
					imageUrl:this.shareImg,
					path: "pages/index/index?h5Url=" + this.sharePath
				}
			}
		}
	}
</script>

<style>
	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.logo {
		height: 200rpx;
		width: 200rpx;
		margin-top: 200rpx;
		margin-left: auto;
		margin-right: auto;
		margin-bottom: 50rpx;
	}

	.text-area {
		display: flex;
		justify-content: center;
	}

	.title {
		font-size: 36rpx;
		color: #8f8f94;
	}
</style>
