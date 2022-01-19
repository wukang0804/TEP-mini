<template>
	<view>
		取到的信息：{{dataJson}}
	</view>
</template>

<script>
	export default {
		onLoad(e) {
			console.log(e)
			this.dataJson = e.data;
		},
		data() {
			return {
				dataJson:'',
			}
		},
		methods: {
			
		},
		mounted() {
			
		}
	}
</script>

<style>

</style>
