<template>
	<view class="p-2 flex align-center
	 border-bottom border-light-secondary" hover-class="bg-light">
		<image :src="item.avatar" 
		style="width: 100rpx;height: 100rpx;"
		class="rounded-circle mr-2"></image>
		<view class="flex flex-column flex-1" style="flex:1;">
			<text class="font-md text-dark">{{item.username}}</text>
			<uni-badge :text="item.age" type="error">
			</uni-badge>
		</view>
		<view class="uni-icon uni-icon-checkbox-filled text-light-muted" :class="item.isFollow ? 'text-light-muted':'text-main'"></view>
	</view>
</template>

<script>
	import uniBadge from '@/components/uni-ui/uni-badge/uni-badge.vue'
	export default {
		components:{
			uniBadge
		},
		props:{
			item:Object,
			index:Number
		}
	}
</script>

<style>
</style>
