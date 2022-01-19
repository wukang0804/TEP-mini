<template>
	<view class="wx-pay-container">

	</view>
</template>

<script>
	export default {
		data() {
			return {
				callBackUrl: '',
				payInfo: {},
			}
		},
		onLoad(options) {
			// 处理支付信息
			this.callBackUrl = options.callBackUrl;
			options.package = decodeURIComponent(options.package);
			options.paySign = decodeURIComponent(options.paySign);
			const {
				success,
				fail,
				cancel
			} = this.PayDoneCallback()
			this.callPayInfo({
				options,
				success,
				fail,
				cancel
			})
		},
		methods: {
			// 发起支付
			callPayInfo(opts) {
				const data = opts.options
				let hasComplete = false;
				wx.requestPayment({
					...data,
					success: function(res) {
						typeof opts.success == 'function' && opts.success()
					},
					fail: function(res) {
						if (res.errMsg == 'requestPayment:fail cancel') {
							typeof opts.cancel == 'function' && opts.cancel();
							hasComplete = true;
							return;
						}
						typeof opts.fail == 'function' && opts.fail()
					},
					complete: function(res) {
						console.log(res)
						if (hasComplete) {
							hasComplete = false;
						} else if (res.errMsg == 'requestPayment:fail cancel' || res.errMsg ==
							"requestPayment:cancel") {
							typeof opts.cancel == 'function' && opts.cancel()
						}
						typeof opts.complete == 'function' && opts.complete(res)
					}
				})
			},
			PayDoneCallback() {
				return {
					success() {
						wx.redirectTo({
							url: `/pages/index/index?payBackType=1&callBackUrl=${this.callBackUrl}`
						});
					},
					fail() {
						wx.redirectTo({
							url: `/pages/index/index?payBackType=2&callBackUrl=${this.callBackUrl}`
						});
					},
					cancel() {
						wx.redirectTo({
							url: `/pages/index/index?payBackType=3&callBackUrl=${this.callBackUrl}`
						});
					}
				}
			}
		}
	}
</script>

<style>

</style>
