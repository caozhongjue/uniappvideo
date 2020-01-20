<template>
	<view>
		<view class="top">
			<view class="row1">
				<view class="title">
					<text class="name">光大银行-快速申请通过</text>
					<text class="price">49.90元</text>
				</view>
				<view class="time">
					<view class="left">截止日期 : <text class="timeleft">2020-01-30</text></view>
					审核 : <text class="timeleft">72小时</text>
				</view>
				<view class="time">
					<view class="left">重复执行 : <text class="timeleft">72小时</text></view>
					剩余 : <text class="timeleft">63次</text>
				</view>
				<view class="shuoming">平均审核用时33小时</view>
			</view>
		</view>
		<view class="top">
			<view class="row2">
				<view class="title">
					<text class="rowtitle">任务说明</text>
				</view>
				<view class="titledetail">
<pre>
1.打开链接
2.实名填写信息
3.申请成功即可
PS：光大银行比较好申请，只要大家认真填写，保证信息真实，基本上都可以通过。</pre>				</view>
			</view>
		</view>
		<view class="top">
			<view class="row3">
				<view class="title">任务步骤</view>
				<view class="shuomingdetial  clearfix">
					<image class="leftimg" src="https://m.jianzhimao.com/static/img/task/icon_step1.png"></image>
					<view class="right clearfix">
						<text>点击打开链接,进入界面后填写资料申请信用即可</text>
						<view class="img">
							<image src="http://img.jianzhimao.com/miaotask/comp/6259790/143f15c6c1f98a941bd6ebea6d61ae47.jpg"></image>
							<image src="http://img.jianzhimao.com/miaotask/comp/6259790/a7e94cc828d5c86490a0475dd15b04c9.jpg"></image>
						</view>
					</view>
				</view>
				<view class="shuomingdetial  clearfix">
					<image class="leftimg" src="https://m.jianzhimao.com/static/img/task/icon_step2.png"></image>
					<view class="right clearfix">
						<text>点击打开链接,进入界面后填写资料申请信用即可</text>
						<view class="img">
							<text>打开链接</text>
						</view>
					</view>
				</view>
				<view class="shuomingdetial  clearfix">
					<image class="leftimg" src="https://m.jianzhimao.com/static/img/task/icon_step3.png"></image>
					<view class="right clearfix">
						<text>请提交申请手机号+姓名</text>
						<view class="img">
							<text>张三+1388888888</text>
						</view>
					</view>
				</view>
				<view class="shuomingdetial  clearfix">
					<image class="leftimg" src="https://m.jianzhimao.com/static/img/task/icon_step4.png"></image>
					<view class="right clearfix">
						<text>请提交申请成功截图+短信审核通过截图</text>
						<view class="img">
							<image src="http://img.jianzhimao.com/miaotask/comp/6259790/a62fd0a97fb3b6440c6ac6fe0de6fedd.jpg"></image>
							<image src="http://img.jianzhimao.com/miaotask/comp/6259790/1f1fb818cc40a7e240d91fdba0d79592.jpg"></image>
						</view>
					</view>
				</view>
			</view>
			
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				detaillist:[]
			}
		},
		onLoad: (option) => {
			console.log(option)
			uni.request({
				url:"https://m.jianzhimao.com/ajax/task/detail?v1579155480316&id="+option,
				method:"GET",
				dataType:"json",
				success: (res) => {
					console.log(res)
				}
			})
		},
		methods: {
			
		}
	}
</script>

<style>
	.clearfix:after{
	     content:".";        
	     display:block;        
	     height:0;        
	     clear:both;        
	     visibility:hidden;        
	
	}
	.top{
		padding-bottom: .5rem ;
		background-color: rgba(239,239,244,1);
	}
	.row1 {
		padding: .5rem;
		
		background-color: #FFFFFF;
	}
	.row1 .title{
		display: block;
		font-weight: 400;
		font-size: .85rem;
		color: #3f3f3f;
		margin-bottom: .5rem;
	}
	.row1 .title .price{
		margin-left: 8px;
		color: #f57400;
	}
	.row1 .time{
		font-size: .7rem;
		line-height: 1.8em;
		color: #a5a5a5;
	}
	.row1 .time .left{
		width: 70%;
		float: left;
	}
	.row1 .time .timeleft{
		color: #3F3F3F;
		margin-left: 8px;
	}
	.row1 .shuoming{
		position: relative;
		height: 1rem;
		margin-top: .5rem;
		width: 100%;
		font-size: .65rem;
		color: #FE7418;
	}
	.row2{
		padding: .5rem;
		background-color: #FFFFFF;
	}
	.row2 .title{
		display: block;
		margin-bottom: .25rem;
		color: #3F3F3F;
		font-size: .75rem;
		font-weight: 400;
		line-height: 1.5em;
	}
	.row2 .titledetail{
		font-size: .75rem;
		line-height: 2em;
		color: #3F3F3F;
		word-wrap: break-word;
		word-break: break-all;
	}
	.row2 .titledetail pre{
		white-space: pre-wrap;
		font-family: inherit;
		font-size: 1em;
	}
	.row3{
		padding: .5rem;
		background-color: #FFFFFF;
	}
	.row3 .title{
		display: block;
		margin-bottom: .25rem;
		color: #3F3F3F;
		font-size: .75rem;
		font-weight: 400;
		line-height: 1.5em;
	}
	.row3 .shuomingdetial{
		position: relative;
		padding-left: 2.4rem;
	}
	.row3 .shuomingdetial .leftimg{
		width: 2.2rem;
		position: absolute;
		left:0;
		top: 6px;
		height: 16px;
	}
	.row3 .shuomingdetial .right{
		position: relative;
		overflow: hidden;
		padding-left: 14px;
		line-height: 1.6;
		padding-bottom: .5rem;
	}
	.row3 .shuomingdetial .right text{
		display: block;
		font-size: 15px;
		color: #3F3F3F;
		font-weight: 400;
		margin-top: 2px;
	}
	.row3 .shuomingdetial .right .img{
		margin: .6rem -.5rem 0 0;
		overflow: hidden;
	}
	.row3 .shuomingdetial .right .img image{
		
		width: 3.2rem;
		height: 3.2rem;
		margin: 0 .5rem .5rem 0;
	}
</style>
