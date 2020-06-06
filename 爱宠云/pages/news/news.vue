<template>
	<view>
		<!--导航-->
		<uni-nav-bar :border="false" :fixed="true" :statusBar="true" @clickRight="openAddInput">
			<view class="flex align-center justify-center font-weight-bold w-100">
				<!--<view class="font-lg text-main mx-1">关注</view>-->
				<view class="mx-1" @click="changeTab(index)"
				v-for="(item,index) in tabBars" :key="index"
				:class="tabIndex === index ? 'font-lg text-main':'font-md text-light-muted'">
					{{item.name}}
				</view>
			</view>
			<view slot="right" class="iconfont icon-ziyuan2"></view>
		</uni-nav-bar>
		
		<swiper :current="tabIndex" :duration="150":style="'height:'+scrollH+'px;'"
		@change="onChangeTable">
			<!--关注-->
			<swiper-item>
				<scroll-view scroll-y="true" :style="'height:'+scrollH+'px;'" 
				@scrolltolower="loadmoreEvent">
					<block v-for="(item,index) in list":key="index">
					<common-list :item="item" :index="index"></common-list>
					<divider></divider>
					</block>
					<load-more :loadmore="loadmore"></load-more>
				</scroll-view>
			</swiper-item>
			
		</swiper>
		
	</view>
</template>

<script>
	const demo =[
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"/static/test.jpg",
			support:{
				type:"support",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"unsupport",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:0,//评论数
			share_num:0//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"/static/test.jpg",
			support:{
				type:"support",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"unsupport",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:3,//评论数
			share_num:2//分享人数
		
		},
		{
			username:"昵称",//昵称
			userpic:"/static/girl.png",//用户头像
			newstime:"2020-5-1 下午 09：23",//发帖时间
			isFollow:true,//是否关注
			title:"我是标题",
			titlepic:"",
			support:{
				type:"",
				support_count:1,
				unsupport_count:2,
				
			},
			comment_count:0,//评论数
			share_num:0//分享人数
		
		},
		
	];
	import uniNavBar from '@/components/uni-ui/uni-nav-bar/uni-nav-bar.vue'
	import commonList from '@/components/common/common-list.vue'
	import loadMore from'@/components/common/load-more.vue'
	export default {
			components:{
				uniNavBar,
				commonList,
				loadMore
			},
		data() {
			return {
				scrollH:500,
				tabIndex:0,
				tabBars:[{
					name:"关注"
				}],
				//关注列表
				list:[],
				//三个上拉加载状态
				loadmore :"上拉加载更多"
			}
		},
		onLoad(){
			uni.getSystemInfo({
				success: res => {
					this.scrollH = res.windowHeight - res.statusBarHeight - 44
				}
			})
			//加载测试数据
			this.list = demo
		},
		methods: {
			//打开发布页
			openAddInput(){
				uni.navigateTo({
					url:'../add-input/add-input',
				});
			},
			//切换选项卡
			changeTab(index){
				this.tabIndex = index
			},
			//滑动
			onChangeTable(e){
				this.tabIndex = e.detail.current
			},
			//顶踩操作
			/*doSupport(e){
				//拿到当前对象
				let item = this.list[e.index]
				let msg = e.type === 'support' ? '顶' : '踩'
				//判断之前是否已经顶踩,若无
				if(item.support.type === ''){
					item.support[e.type+'_count']++
				}else if(item.support.type === 'support' && e.type === 'unsupport'){
					//之前顶了
					item.support.support_count--;
					item.support.unsupport_count++;
				}else if(item.support.type === 'unsupport' && e.type === 'support'){
					//之前踩了
					item.support.support_count++;
					item.support.unsupport_count--;
				}
				item.support.type = e.type
				uni.showToast({title: msg + '成功'});
			},*/
			//上拉加载更多
			loadmoreEvent(){
				//判断当前是否处于可加载状态
				if (this.loadmore !== '上拉加载更多') return;
				// 设置加载状态
				this.loadmore = '加载中...'
				//模拟请求数据
				setTimeout(()=>{
					//加载数据
					this.list = [...this.list,...this.list]
					//设置加载状态
					this.loadmore = '上拉加载更多'
				},2000)
			}
		}
	}
</script>

<style>

</style>
