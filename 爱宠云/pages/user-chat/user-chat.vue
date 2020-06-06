<template>
	<view>
		<!-- 聊天列表-->
		<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'">
			<block v-for="(item,index) in list" :key="index">
				<user-chat-list :item="item" :index="index"></user-chat-list>
			</block>
		</scroll-view>
		<!-- 底部操作条-->
		<view style="height: 100rpx;"
		class="fixed-bottom flex align-center border-top bg-white">
			<input type="text" v-model="content"
			 class="flex-1 rounded bg-light mr-2" 
			 style="padding:5rpx   ;" placeholder="输入文字"/>
			<view class="iconfont icon-fenxiang flex align-center
			 justify-center font-lg animated" hover-class="jello text-main" 
			 style="width: 100rpx;" @click="submit"></view>
		</view>
		
	</view>
</template>

<script>
	//模拟当前用户的id
	const uid = 1;
	import userChatList from '@/components/user-chat/user-chat-list.vue'
	export default {
		components:{
			userChatList
		},
		data() {
			return {
				content:"",
				scrollH:500,
				list:[{
					user_id:2,
					avatar:"/static/girl.png",
					username:"昵称",
					data:"123132",
					type:"text",
					create_time:"12:30"
				},{
					user_id:1,
					avatar:"/static/girl.png",
					username:"昵称",
					data:"123132",
					type:"text",
					create_time:"12:30"
				}]
			}
		},
		onLoad(){
			uni.getSystemInfo({
				success: (res) => {
					this.scrollH = res.windowHeight - uni.upx2px(101)
				}
			})
		},
		methods: {
			//发送消息
			submit(){
				let obj = {
					user_id:1,
					avatar:"/static/girl.png",
					username:"昵称",
					data:this.content,
					type:"text",
					create_time:"12:30"
				}
				if (this.content === ''){
					return uni.showToast({
						title:'消息不能为空',
						icon:'none'
					});
				}
				this.list.push(obj)
			}
		}
	}
</script>

<style>

</style>
