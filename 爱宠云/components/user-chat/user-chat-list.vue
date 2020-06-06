<template>
	<!-- 左边-->
	<view class="flex align-start my-2"
	:style="item.user_id === 1 ? 'flex-direction: row-reverse;' : ''">
		<image :src="item.avatar"
		 style="width: 100rpx;height: 100rpx;"
		 class="rounded-circle"></image>
		<view class="bg-light p-2 rounded mx-2"
		style="max-width: 400rpx;min-width: 100rpx;">
		{{item.data}}
		</view>
	</view>

</template>

<script>
	//模拟当前用户的id
	const uid = 1;
	export default {
		props:{
			item:Object,
			index:Number
		},
		computed:{
			//是否是用户本人
			isSelf(){
				return uid === this.item.user_id
			}
		}
	}
</script>

<style>
</style>
