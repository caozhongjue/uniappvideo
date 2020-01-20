<template>
	<scroll-view class="scroll-view" show-scrollbar="true" scroll-y="true" @scrolltoupper="scroll" @scrolltolower="lower"
	 :style="{height:scrollHeight+ 'px'}">
		<view class="content">

			<view class="banner">
				<swiper class="swiper" :indicator-dots="true" indicator-active-color="rgba(30,144,255,.5)" :autoplay="true"
				 :interval="5000" :duration="1000" :circular="true">
					<swiper-item>
						<image class="lunbo" src="../../static/carousel/lirongfa_1576746467006.jpg"></image>
					</swiper-item>
					<swiper-item>
						<image class="lunbo" src="../../static/carousel/batmanvssuperman.png"></image>
					</swiper-item>
					<swiper-item>
						<image class="lunbo" src="../../static/carousel/spiderman.png"></image>
					</swiper-item>
				</swiper>
				<view class="classic">
					<view class="clumn" @tap="openinfo(1)">
						<view class="img">
							<image src="../../static/carousel/icon_hot.png"></image>
						</view>
						<text>热招兼职</text>
					</view>
					<view class="clumn" @tap="openinfo(2)">
						<view class="img">
							<image src="../../static/carousel/icon_direct.png"></image>
						</view>
						<text>直招兼职</text>
					</view>
					<view class="clumn" @tap="openinfo(3)">
						<view class="img">
							<image src="../../static/carousel/icon_miaomission.png"></image>
						</view>
						<text>喵任务</text>
					</view>
					<view class="clumn">
						<view class="img">
							<image src="../../static/carousel/icon_luyong.png"></image>
						</view>
						<text>鹿用全职</text>
					</view>
				</view>
				<view class="guanggao">
					<image src="../../static/carousel/jzmt.png"></image>
				</view>
				<view v-for="item in list" class="row" v-bind:id="item.id">
					<view class="row1 clearfix">
						<text v-text="item.title" class="left"></text>
						<text class="right" v-text="item.unit"></text><text v-text="item.salary" class="right"></text>
					</view>
					<view class="row2">
						<text v-text="item.area"></text> <text v-text="item.clearing_cycle"></text>
					</view>
					<view class="row3 clearfix">
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
				</view>
				{{loadingText}}
				</view>

			</view>

		</view>
	</scroll-view>
</template>

<script>
	export default {
		data() {
			return {
				title: 'Hello',
				list: [],
				pagination: 1,
				loadingText: '上拉加载更多',
				ph: 0,
				scrollHeight: 0,
				showanimation: false
			}
		},
		onLoad() {
			let that = this
			uni.getSystemInfo({
				success: function(res) { // res - 各种参数
					that.scrollHeight = res.windowHeight
					console.log("onload"+that.scrollHeight)
				}
			});
			this.clickdetail()
		},
		onReady() {
			let that=this;
			uni.getSystemInfo({ //调用uni-app接口获取屏幕高度
				success(res) { //成功回调函数
					that.ph=res.windowHeight //windoHeight为窗口高度，主要使用的是这个
					console.log("onready"+that.ph)
					let titleH=uni.createSelectorQuery().select(".scroll-view"); //想要获取高度的元素名（class/id）
					titleH.boundingClientRect(data=>{
						that.scrollHeight=data.height  //计算高度：元素高度=窗口高度-元素距离顶部的距离（data.top）
						console.log("onready"+that.scrollHeight)
					}).exec()
				}
			})
			this.clickdetail()
		},
		methods: {
			scroll() {
				uni.startPullDownRefresh();
			},
			onPullDownRefresh() {
				this.pagination = 1;
				this.clickdetail();
				setTimeout(function() {
					uni.stopPullDownRefresh();
				}, 1000);
			},
			clickdetail: function(e) {
				uni.request({
					url: "https://m.jianzhimao.com/ajax/get-job-list?page=" + this.pagination +
						"&prov=&city=%E5%B9%BF%E5%B7%9E&area=&type=&time=",
					// url: "https://m.jianzhimao.com/job/pageDirect?page=" + this.pagination + "&city=%E5%B9%BF%E5%B7%9E", //仅为示例，并非真实接口地址。
					method: "GET",

					dataType: 'json',
					success: (res) => {
						this.list = res.data.data.list

					}
				});
			},
			lower: function(e) {
				this.pagination = this.pagination + 1
				this.showanimation=true
				this.loadingText = '加载中'
				uni.request({
					url: "https://m.jianzhimao.com/ajax/get-job-list?page=" + this.pagination +
						"&prov=&city=%E5%B9%BF%E5%B7%9E&area=&type=&time=",
					method: "GET",

					dataType: 'json',
					success: (res) => {

						if (res.data.data.length <= 0) {
							this.loadingText = '我是有底线的'
							this.showanimation=false
							return false
						}
						this.list = this.list.concat(res.data.data.list),
							this.loadingText = '上拉加载更多'
							this.showanimation=false

					}
				});

			},
			openinfo(e) {
				console.log(e)
				if (e == 1) {
					uni.navigateTo({
						url: '../common/common',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
				if (e == 2) {
					uni.navigateTo({
						url: '../zhizhao/zhizhao',
						success: res => {},
						fail: () => {},
						complete: () => {}
					});
				}
				if (e == 3) {
					uni.navigateTo({
						url: '../miaorenwu/miaorenwu',
						success: res => {},
						fail: () => {},
						complete: () => {}
					})
				}

			}
		}
	}
</script>

<style>
	.clearfix:after {
		content: ".";
		display: block;
		height: 0;
		clear: both;
		visibility: hidden;

	}

	.content {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
	}

	.banner {
		width: 100%;
		height: auto;
	}

	.banner .lunbo {
		width: 100%;
		height: 400upx;
	}

	.banner .classic {
		display: flex;
		justify-content: space-between;
		align-items: center;
		padding: 0 1rem;
		padding-top: .8rem;
		padding-bottom: .6rem;
	}

	.banner .classic .clumn {
		text-align: center;
	}

	.banner .classic .clumn .img {
		width: 2.2rem;
		height: 2.2rem;
		border-radius: 50%;
		overflow: hidden;
		text-align: center;
		display: inline-block;
	}

	.banner .classic .clumn .img image {
		width: 2.2rem;
		height: 2.2rem;
	}

	.banner .classic .clumn text {
		font-size: .6rem;
		color: rgba(51, 51, 51, 1);
		line-height: 1.5;
		margin-top: .4rem;
		display: block;
	}

	.banner .guanggao {
		width: 100%;
		margin: 0;
		z-index: 1;
		border-bottom: .6rem solid #eee;
		box-sizing: content-box;
		height: 5rem;
	}

	.banner .guanggao image {
		height: 5rem;
		width: 100%;
	}

	.banner .row {
		border-bottom: 1px solid #ddd;
		padding: 20.5px;
	}

	.banner .row .row1 {
		line-height: 1.1rem;
	}

	.banner .row .row1 .left {
		width: 74%;
		overflow: hidden;
		text-overflow: ellipsis;
		display: -webkit-box;
		-webkit-box-orient: vertical;
		-webkit-line-clamp: 1;
		float: left;
	}

	.banner .row .row1 .right {
		float: right;
		color: #ff660d;
		text-align: center;
	}

	.banner .row .row2 {
		font-size: .6rem;
		margin: .4rem 0 0;
		color: #9c9c9c;
	}

	.banner .row text {
		font-size: 14px;
	}

	.banner .row .row2 text:first-child {
		margin-right: 10px;
	}

	.banner .row .row3 text {
		font-size: .55rem;
		margin-top: .4rem;
		color: #999999;
		padding: .15rem .2rem;
		margin-right: .8rem;
		border-radius: 4px;
		background-color: rgba(245, 245, 245, 1);
	}

	.banner .row .row3 image {
		width: 2rem;
		height: 1rem;
		vertical-align: middle;
		float: right;
	}

	.banner .loadmoring {
		font-size: 15px;
		color: #555555;
		text-align: center;
		height: 40px;
		line-height: 40px;
	}

	.banner .loadmored {
		font-size: 15px;
		color: #555555;
		text-align: center;
		height: 40px;
		line-height: 40px;
		;
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
		animation-delay: 0.3s;
	}
	
	.demo3:nth-child(6) {
		left: 6px;
		top: 23px;
		animation-delay: 0.4s;
	}
	
	.demo3:nth-child(7) {
		left: 2px;
		top: 15px;
		animation-delay: 0.5s;
	}
	
	.demo3:nth-child(8) {
		left: 6px;
		top: 6px;
		animation-delay: 0.6s;
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
