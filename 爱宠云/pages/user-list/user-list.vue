<template>
	<view>
		<!--tab-->
		<view class="flex align-center py-2">
			<view class="flex-1 flex align-center justify-center"
			v-for="(item,index) in tabBars" :key="index"
			:class="index === tabIndex ? 'font-lg font-weight-bold text-main':'font-md'"
			@click="changeTab(index)">
			{{item.name}} <text v-if="item.num > 0" class="mr-2"> {{item.num}}</text>
			</view>
		</view>
		<!--列表-->
		
		<swiper :duration="150" :current="tabIndex" @change="onChangeTab"
		:style="'height:'+scrollH+'px;'">
			<swiper-item v-for="(item,index) in newsList" :key="index">
				<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'"
				@scrolltolower="loadmore(index)">
					<template v-if="item.list.length > 0">
					<!--列表-->
					<block v-for="(item2,index2) in item.list" :key="index2">
						<!--列表样式-->
						<user-list :item="item2" :index="index"></user-list>
					</block>
					<!--上拉加载-->
					<load-more :loadmore="item.loadmore"></load-more>
					</template>
					<!--无数据-->
					<template v-else>
						<no-thing></no-thing>
					</template>
				</scroll-view>
			</swiper-item>
		</swiper>
		
	</view>
</template>

<script>
	
	const demo = [{
		avatar:"/static/girl.png",
		username:"昵称",
		age:24,
		isFollow:true
	},{
		avatar:"/static/girl.png",
		username:"昵称",
		age:24,
		isFollow:true
	},{
		avatar:"/static/girl.png",
		username:"昵称",
		age:24,
		isFollow:true
	},{
		avatar:"/static/girl.png",
		username:"昵称",
		age:24,
		isFollow:true
	},{
		avatar:"/static/girl.png",
		username:"昵称",
		age:24,
		isFollow:true
	}]
	
	import noThing from '@/components/common/no-thing.vue'
	import loadMore from'@/components/common/load-more.vue'
	import userList from '@/components/user-list/user-list.vue'
	export default {
		components:{
			noThing,
			loadMore,
			userList
		},
		data() {
			return {
				//列表高度
				scrollH:600,
				tabIndex:0,
				tabBars:[{
					name:"互关",
					num:0
				},{
					name:"关注",
					num:2
				},{
					name:"粉丝",
					num:30
				}],
				
				newList:[]
			}
		},
		//监听点击输入框事件
		onNavigationBarSearchInputClicked() {
			uni.navigateTo({
				url:'../search/search',
			})
		},
		onNavigationBarButtonTap() {
			uni.navigateBack({
				delta: 1
			})
		},
		onLoad() {
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - uni.upx2px(101)
				}
			})
			//根据选项生成列表
			this.getData()
		},
		methods: {
			//获取数据
			getData(){
				var arr=[]
				for (let i = 0; i < this.tabBars.length; i++) {
					//生成列表模版
					let obj = {
						//1.上拉加载更多2.加载中...3.没有更多了
						loadmore:"上拉加载更多",
						list:[]
					}
					if (i < 2){
						obj.list=demo
					}
					arr.push(obj)
				}
				this.newsList=arr
			},
			//tab切换
			changeTab(index){
				this.tabIndex = index
			},
			onChangeTab(e){
				this.changeTab(e.detail.current)
			},
			//上拉加载更多
			loadmore(index){
				let item = this.newsList[index]
				//判断是否处于可加载状态
				if(item.loadmore !== '上拉加载更多') return;
				//修改当前列表的加载状态
				item.loadmore = '加载中...'
				//模拟数据请求
				setTimeout(()=>{
					//加载数据
					item.list = [...item.list,...item.list]
					//恢复加载状态
					item.loadmore = '上拉加载更多'
				},1000)
			}
		}
	}
</script>

<style>

</style>
