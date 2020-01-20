<template>
	<view>
		<scroll-view scroll-y="true" @scrolltolower="lower" v-bind:style="{height:scrollHeight+'px'}">
			<view>
				<view class="row1 clearfix">
					<view class="left">
						<image src="http://img.jianzhimao.com/activity/prize/20190407/a19a2efb076f3713117e00b6966904e9.jpg"></image>
					</view>
					<view class="right">
						<view class="top">
							<image src="http://img.jianzhimao.com/activity/prize/20190430/d7480e38576a21db2072c9cf45989042.jpg"></image>
						</view>
						<view class="bottom">
							<image class="left" src="http://img.jianzhimao.com/activity/prize/20190221/5ca7b0d76ee7765a766dd4d62aa5888a.jpg"></image>
							<image class="right" src="http://img.jianzhimao.com/activity/prize/20191224/838c154770b7e58654bfcdd09b144de4.png"></image>
						</view>
					</view>

				</view>
				<view class="row22">
					<view class="row2">
						<view class="left slt" @click="click1(1)" :class="{active:1==currentIndex}">
							<text id="classify">分类</text>
						</view>
						<view class="right slt" @click="click2(2)" :class="{active:2==currentIndex}">
							<text id="sort">排序</text>
						</view>
					</view>
					<view v-if="currentIndex ==1" class="loc_box_con" id="loc_box_con">
						<view @click="clicklist(1)" :class="{active: 1==clicklistIndex} ">不限</view>
						<view @click="clicklist(2)" :class="{active: 2==clicklistIndex}">注册体验</view>
						<view @click="clicklist(3)" :class="{active: 3==clicklistIndex}">社交传播</view>
						<view @click="clicklist(4)" :class="{active: 4==clicklistIndex}">调查采集</view>
						<view @click="clicklist(5)" :class="{active: 5==clicklistIndex}">办信用卡</view>
						<view @click="clicklist(6)" :class="{active: 6==clicklistIndex}">其他</view>
					</view>
					<view v-if="currentIndex ==2" class="m-sxsqe" id="m-sxsqe">
						<view @click="clicklist2(1)" :class="{active: 1==clicklistIndex2}">最新发布</view>
						<view @click="clicklist2(2)" :class="{active: 2==clicklistIndex2}">单价最高</view>
						<view @click="clicklist2(3)" :class="{active: 3==clicklistIndex2}">综合排序</view>
					</view>
				</view>
				<view v-if="currentIndex ==1 || currentIndex ==2" class="m-loc-fixed" id="m-loc-fixed"></view>
				<view class="list clearfix" v-for="item in work" @tap="listdetail(item.id)">
					<image v-if="item.category_1=='1'" src="https://img.jianzhimao.com/static/m3/static/css/img/task/icon_register.png?v=43c1aa1"></image>
					<image v-if="item.category_1=='2'" src="https://img.jianzhimao.com/static/m3/static/css/img/task/icon_spread.png?v=73fa6fb"></image>
					<image v-if="item.category_1=='3'" src="https://img.jianzhimao.com/static/m3/static/css/img/task/icon_survey.png?v=da14a09"></image>
					<image v-if="item.category_1=='4'" src="https://img.jianzhimao.com/static/m3/static/css/img/task/icon_creditcard.png?v=3fc2b13"></image>
					<image v-if="item.category_1=='5'" src="../../static/carousel/icon_other.png"></image>
					<view class="right">
						<view class="listleft">
							<text class="title" v-text="item.title">浏览精选推荐广告</text>
							<view class="view1">剩余<text class="datali" v-text="item.task_min_num">399</text>次<text class="time" v-text="item.create_time"></text></view>
							<view v-if="item.labels != null" v-for="listitem in item.labels"><text class="tagli" v-text="listitem.name"
								 :style="{'background-color': listitem.color}">置顶</text></view>
						</view>
						<view class="listright">
							<view><text class="num" v-text="item.price">0.4</text>元</view>
							<text class="datali" v-text="item.avg_review_time">3小时审核</text>
						</view>
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
					</view>{{loadingText}}
				</view>
			</view>
		</scroll-view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				classify: '',
				currentIndex: 0,
				scrollHeight: 0,
				loadingText: '上拉加载更多',
				pagination: 1,
				work: [],
				clicklistIndex: 1,
				clicklistIndex2: 3,
				showanimation: false
			}
		},
		onLoad() {
			var _this = this
			uni.getSystemInfo({
				success(res) {
					_this.scrollHeight = res.windowHeight
				}
			})
			uni.request({
				url: "https://m.jianzhimao.com/ajax/tasks?province=&city=%E5%B9%BF%E5%B7%9E&type=&category_1=&order=4&page=" +
					this.pagination + "&pageSize=20&channel_id=&from=",
				method: "GET",
				dataType: "json",
				success: (res) => {
					this.work = res.data.data
				}
			})
		},
		methods: {
			lower() {
				this.pagination = this.pagination + 1
				this.loadingText = '加载中'
				this.showanimation = true
				uni.request({
					url: "https://m.jianzhimao.com/ajax/tasks?province=&city=%E5%B9%BF%E5%B7%9E&type=&category_1=&order=4&page=" +
						this.pagination + "&pageSize=20&channel_id=&from=",
					method: "GET",
					dataType: "json",
					success: (res) => {
						if (res.data.data.length <= 0) {
							this.loadingText = '我是有底线的'
							this.showanimation = false
							return false
						}
						this.work = this.work.concat(res.data.data),
							this.loadingText = '上拉加载更多'
						this.showanimation = false
					}
				})
			},
			listdetail(e) {
				uni.navigateTo({
					url: '../detail/detail?id=' + e,
					success: res => {},
					fail: () => {},
					complete: () => {}
				})
			},
			click1(index) {
				if (this.currentIndex == 0 || this.currentIndex == 2) {
					this.currentIndex = index
				} else {
					this.currentIndex = 0
				}
			},
			click2(index) {
				if (this.currentIndex == 0 || this.currentIndex == 1) {
					this.currentIndex = index
				} else {
					this.currentIndex = 0
				}
			},
			clicklist(index) {
				this.clicklistIndex = index
				this.currentIndex = 0
			},
			clicklist2(index) {
				this.clicklistIndex2 = index
				this.currentIndex = 0
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

	.row1 .left {
		width: 41.33%;
		float: left;
	}

	.row1 .left image {
		width: 100%;
		height: 135px;
		display: block;
	}

	.row1 .right {
		float: right;
		width: 58.67%;
	}

	.row1 .right .top,
	.row1 .right .top image {
		width: 100%;
		height: 67.5px;
	}

	.row1 .right .bottom,
	.row1 .right .bottom image {
		width: 100%;
		height: 67.5px;
	}

	.row1 .right .bottom .left,
	.row1 .right .bottom .right {
		width: 50%;
	}

	.row22 {
		background-color: rgb(239, 239, 244, 1);
		padding-bottom: 8px;
		position: relative;
	}

	.row22 .loc_box_con {
		display: block;
		position: absolute;
		top: 2.2rem;
		left: 0;
		width: 100%;
		z-index: 9;
		background-color: #FFFFFF;
	}

	.row22 .loc_box_con view {
		color: #333333;
		font-size: .8rem;
		width: 100%;
		height: 2.2rem;
		line-height: 2.2rem;
		text-align: left;
		border-bottom: 1px solid #efefef;
		list-style: none;
		text-indent: 1rem;
		position: relative;
	}

	.row22 .loc_box_con view.active::before {
		content: "";
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/cur.png?v=cbc9a46) right center no-repeat;
		height: 100%;
		width: 1.8rem;
		background-size: .85rem;
		position: absolute;
		right: 1rem;
		top: 0;
	}

	.row22 .m-sxsqe {
		display: block;
		position: absolute;
		top: 2.2rem;
		left: 0;
		width: 100%;
		z-index: 9;
		background-color: #FFFFFF;
	}

	.row22 .m-sxsqe view {
		color: #333333;
		font-size: .8rem;
		width: 100%;
		height: 2.2rem;
		line-height: 2.2rem;
		text-align: left;
		border-bottom: 1px solid #efefef;
		list-style: none;
		text-indent: 1rem;
		position: relative;
	}

	.row22 .m-sxsqe view.active::before {
		content: "";
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/cur.png?v=cbc9a46) right center no-repeat;
		height: 100%;
		width: 1.8rem;
		background-size: .85rem;
		position: absolute;
		right: 1rem;
		top: 0;
	}

	.m-loc-fixed {
		display: block;
		top: 225px;
		position: fixed;
		bottom: 0;
		right: 0;
		z-index: 8;
		background: #000000;
		opacity: .5;
		width: 100%;
		height: 100%;
	}

	.row2 {
		display: -webkit-box;
		position: relative;
		background-color: rgb(255, 255, 255);
	}

	.row2 .slt.active {
		color: #FE7418;
	}

	.row2 .left,
	row2 .right {
		position: relative;
	}

	.row2 .left text:after {
		width: 14px;
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/sel_type.png?v=c10a641) no-repeat 0 center;
		background-size: 28px;
		background-color: white;
	}

	.row2 .left.active text:after {
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/sel_type.png?v=c10a641) no-repeat right center;
		background-size: 28px;
	}

	.row2 .left text:after,
	.row2 .right text:after {
		content: '';
		position: absolute;
		top: 0;
		left: 62px;
		height: 100%;
		overflow: hidden;
	}

	.row2 .right text:after {
		width: 14px;
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/sel_sqe.png?v=6564b18) no-repeat 0 center;
		background-size: 30px;
	}

	.row2 .right.active text:after {
		background: url(https://img.jianzhimao.com/static/m3/static/css/img/sel_sqe.png?v=6564b18) no-repeat right center;
		background-size: 30px;
	}

	.row2 .slt {
		-webkit-box-flex: 1;
		position: relative;
		height: 2.2rem;
		line-height: 2.2rem;
		font-size: .8rem;
		text-align: center;
		padding: 0 .3rem;
		overflow: hidden;
		white-space: nowrap;
		text-overflow: ellipsis;
		box-sizing: border-box;
	}

	.list {
		padding: .7rem .6rem .7rem 3rem;
		position: relative;
	}

	.list image {
		width: 2rem;
		height: 2rem;
		display: block;
		left: .5rem;
		margin-top: -1rem;
		top: 50%;
		position: absolute;
	}

	.list .right {
		position: relative;
	}

	.list .right .listleft {
		padding-right: 3.6rem;
	}

	.list .right .listleft .title {
		font-size: .8rem;
		color: #333;
		font-weight: 400;
		display: block;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}

	.list .right .listleft .view1,
	.list .right .listright view {
		font-size: .65rem;
		color: #919191;
		line-height: 1.4em;

	}

	.list .right .listleft .view1 .time {
		margin-left: 3%;
	}

	.list .right .listleft .datali,
	.list .right .listright .datali {
		font-size: .65rem;
		color: #919191;
		line-height: 1.4rem;
		padding-top: 6px;
	}

	.list .right .listleft .tagli {
		background-color: #fe7418;
		line-height: .75rem;
		height: .75rem;
		padding: 0 .125rem;
		border-radius: 2px;
		margin-right: .1rem;
		color: #fff;
		font-size: .55rem;
	}

	.list .right .listright {
		position: absolute;
		top: 0;
		right: 0;
		text-align: right;
		width: 4.5rem;
	}

	.list .right .listright .num {
		color: #fe7418;
		font-size: .9rem;
	}

	.list .right .listright text {

		font-size: 15px;
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
			transform: scale(3);
		}
	}

	@-webkit-keyframes demo3 {

		0%,
		40%,
		100% {
			transform: scale(1);
		}

		20% {
			transform: scale(3);
		}
	}
</style>
