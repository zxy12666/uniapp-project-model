<template>
	<view class="content">
		<view class="title">{{show.title}}</view>
		<view class=" author">{{show.author_name}}{{show.created_at}}</view>
		<rich-text class="newscontent" :nodes="show.content"></rich-text>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				show: []
			}
		},
		onLoad:function(e){
			uni.showLoading({
				title: '加载中'
			}),
			uni.request({
				url: 'https://unidemo.dcloud.net.cn/api/news/36kr/' + e.newsid,
				method: 'GET',
				data: {},
				success: res => {
					this.show = res.data;
					uni.hideLoading();
				},
				fail: () => {},
				complete: () => {}
			});
		},
		methods: {
			
		}
	}
</script>

<style>
.content{
	width: 96%;
	padding: 5upx 2%;
}

.title{
	line-height: 1.5em;
	font-size: 20px;
	font-weight: bold;
	text-align: center;
    margin: 15upx;
}

.time{
	width: 100%;
}

.border{
	display: inline-block;
	width: 10%;
	height: 0;

	border-bottom: #888888 solid 1px;
}

.author{
	/* display: inline-block; */
	/* width: 80%; */
	line-height: 50px;
	font-size: 14px;
	color: #888888;
	text-align: center;
}

.newscontent{
	font-size: 16px;
	line-height: 1.5em;
}
</style>
