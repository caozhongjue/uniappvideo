<template>
	<view>
		<scroll-view show-scrollbar="true" scroll-y="true" @scrolltolower="lower" :style="{height:scrollHeight+ 'px'}">
		<view>
			<view class="row1">
				<image class="img" src="../../static/carousel/direct_job.png"></image>
			</view>
			<view class="list" v-for="item in work" v-bind:id="item.id" v-bind:key="item.id">
				<view class="list1">
					<text class="left" v-text="item.title"></text>
					<text class="right" v-text="item.salary"></text>
				</view>
				<view class="list2">
					<text v-text="item.area"></text>
					<text v-text="item.clearing_cycle"></text>
				</view>
				<view class="row3">
					<text v-if="item.welfare != null" v-for="listitem in item.welfare" v-text="listitem"></text>
					<image v-if="item.hot==1" src="https://img.jianzhimao.com/static/m3/static/css/img/icon_remen.png?v=1ba2c70"></image>
				</view>
			</view>
			<view class="loadmoring">
			<view class="loader" v-if="showanimation==true">
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
				<view class="demo3"></view>
			</view>{{loadingText}}</view>
		</view>
		</scroll-view>
		
	</view>
</template>

<script>
	export default {
		data() {
			return {
				pagination: 1,
				work: [],
				loadingText: '上拉加载更多',
				scrollHeight: 0,
				showanimation: false,
				uri: 'https://m.jianzhimao.com'
			}
		},
		onLoad() {
			let _this = this
			uni.getSystemInfo({
				success(res) {
					_this.scrollHeight = res.windowHeight
				}
			})
			uni.request({
				url: this.uri+"/job/pageDirect?page=" + this.pagination + "&city=%E5%B9%BF%E5%B7%9E",
				method: "GET",
				dataType: "json",
				success: (res) => {
					console.log(res)
					this.work = res.data.data
					console.log(this.work)
				}
			})
			
			
		},
		methods: {
			lower() {
				this.pagination = this.pagination + 1
				this.loadingText = '加载中'
				this.showanimation=true
				uni.request({
					url: this.uri+"/job/pageDirect?page=" + this.pagination + "&city=%E5%B9%BF%E5%B7%9E",
					method: "GET",
					dataType: 'json',
					success: (res) => {
						if (res.data.data.length <= 0) {
							this.loadingText = '我是有底线的'
							this.showanimation=false
							return false
						}
						this.work = this.work.concat(res.data.data),
							this.loadingText = '上拉加载更多'
							this.showanimation=false
					}
				});
			}
		}
	}
</script>

<style>
	.row1 {
		width: 100%;
		height: 125px;
	}

	.row1 .img {
		width: 100%;
		height: 125px;
	}

	.list {
		padding: .8rem;
		position: relative;
		overflow: hidden;
		border-bottom: 1px solid #DDDDDD;
	}

	.list .list1 {
		font-size: .8rem;
		color: #333333;
		overflow: hidden;
		text-overflow: ellipsis;
		font-weight: 400;
		white-space: nowrap;
		line-height: 1.13rem;
	}

	.list .list1 .left {
		display: inline-block;
		width: 75%;
		overflow: hidden;
		text-overflow: ellipsis;
		vertical-align: middle;
	}

	.list .list1 .right {
		float: right;
		color: #ff660D;
	}

	.list .list2 {
		font-size: .6rem;
		color: #9C9C9C;
		width: 100%;
		margin-top: .4rem;
	}

	.list .list2 text:first-child {
		margin-right: 5px;
	}
	.row3 text{
		font-size: .55rem;
		margin-top: .4rem;
		color: #999999;
		padding: .15rem .2rem;
		margin-right: .8rem;
		border-radius: 4px;
		background-color: rgba(245,245,245,1);
	}
	.loadmoring {
		font-size: 15px;
		color: #555555;
		text-align: center;
		height: 40px;
		line-height: 40px;
	}
	
	.loader {
		position: relative;
		width: 30px;
		height: 30px;
		display: inline-block;
		vertical-align: middle;
		margin-right: 10px;
	}
	.demo3 {
		width: 4px;
		height: 4px;
		border-radius: 2px;
		background: #68b2ce;
		position: absolute;
		animation: demo3 linear 0.8s infinite;
		-webkit-animation: demo3 linear 0.8s infinite;
	}
	
	.demo3:nth-child(1) {
		left: 15px;
		top: 2px;
		animation-delay: 0s;
	}
	
	.demo3:nth-child(2) {
		left: 24px;
		top: 6px;
		animation-delay: 0.1s;
	}
	
	.demo3:nth-child(3) {
		left: 27px;
		top: 15px;
		animation-delay: 0.1s;
	}
	
	.demo3:nth-child(4) {
		left: 23px;
		top: 24px;
		animation-delay: 0.2s;
	}
	
	.demo3:nth-child(5) {
		left: 15px;
		top: 26px;
		animation-delay: 0.4s;
	}
	
	.demo3:nth-child(6) {
		left: 6px;
		top: 23px;
		animation-delay: 0.5s;
	}
	
	.demo3:nth-child(7) {
		left: 2px;
		top: 15px;
		animation-delay: 0.6s;
	}
	
	.demo3:nth-child(8) {
		left: 6px;
		top: 6px;
		animation-delay: 0.7s;
	}
	
	@keyframes demo3 {
	
		0%,
		40%,
		100% {
			transform: scale(1);
		}
	
		20% {
			transform: scale(2);
		}
	}
	
	@-webkit-keyframes demo3 {
	
		0%,
		40%,
		100% {
			transform: scale(1);
		}
	
		20% {
			transform: scale(2);
		}
	}
</style>
