<template>
	<joy-page :joyPageStatus="joyPageStatus">
		<template v-if="status == '暂无数据'">
			<m-empty-data :coverUrl="no_order_1" noTxt="暂无相关记录"></m-empty-data>
		</template>
		<template v-else>
			<view class="uni-list">
				<view class="uni-list-cell" hover-class="uni-list-cell-hover" v-for="(item,index) in news" :key="index">
					<view class="uni-media-list" @tap="show" :data-newsid="item.post_id">
						<image class="uni-media-list-logo" :src="item.author_avatar"></image>
						<view class="uni-media-list-body">
							<view class="uni-media-list-text-top">{{item.title}}</view>
							<view class="uni-flex uni-row">
								<view class="uni-flex-item uni-flex-item uni-media-list-text-bottom uni-ellipsis">{{item.author_name}}</view>
								<view class="uni-flex-item uni-flex-item time">{{item.created_at}}</view>
							</view>
						</view>
					</view>
				</view>
				<view class="load-more-box">
					<uni-load-more v-if="status == '请求中'" status="正在加载..." :showIcon="true"></uni-load-more>
					<uni-load-more v-if="status == '没有更多'" status="没有更多了" :showIcon="false"></uni-load-more>
					<uni-load-more v-if="status == '请求失败'" status="加载失败，点我重试" :showIcon="false" @click="reLoad"></uni-load-more>
				</view>
			</view>
		</template>
	</joy-page>
</template>

<script>
	import uniLoadMore from "@/components/uni-load-more/uni-load-more.vue"
	import mEmptyData from "@/components/m-empty-data/m-empty-data.vue"
	
	export default {
		components: {
			uniLoadMore,
			mEmptyData
		},
		data() {
			return {
				news: [],
				joyPageStatus: "loading",
				status: "",
				query: {
					pageSize: 20,
					pageNum: 1
				},
				nomore: false
			}
		},
		created: function() {
			uni.setNavigationBarTitle({
					title: "新闻首页"
				}),
				this.fresh();
			this.joyPageStatus = "success";
		},
		async onPullDownRefresh(){
			await this.fresh(); //执行刷新程序
			uni.stopPullDownRefresh();
		},
		onReachBottom() {
			if(!this.nomore){
				this.fresh(); //执行刷新程序
			}
		},
		methods: {
			show(e) {
					this.$mRouter.push({
					route: this.$mRoutesConfig.articleInfo,
					query: {
						newsid: e.currentTarget.dataset.newsid
					}
				})
			},
			async fresh() {
				try {
					this.status = "请求中";
					let data = await this.$apis.postNewList(this.query);
					this.news = this.news.concat(data || []);
					this.changeStatus(data);
				} catch (error) {
					this.status = "请求失败";
				}
			},
			// 修改请求状态
			changeStatus(data) {
				if (this.news.length === 0) {
					this.status = "暂无数据";
				} else if (this.query.pageNum * this.query.pageSize > this.news.length) {
					this.nomore = true;
					this.status = "没有更多";
				} else {
					this.status = "请求更多";
				}
			},
		}
	}
</script>

<style lang="scss" scoped>
	.content {
		text-align: center;
		height: auto;
	}

	.uni-media-list-body {
		height: auto;
	}

	.uni-media-list-text-bottom {
		width: 50%;
		display: inline-block;
		text-align: left;
		float: left;
	}

	.time {
		display: inline-block;
		width: 50%;
	}

	.uni-row {
		width: 100%;

	}

	.uni-flex-item {
		line-height: 30upx;
	}

	.uni-media-list-text-top {
		text-align: left;
	}
	.item {
		display: flex;
		background: #fff;
		padding: 20upx;
		flex-direction: column;
		margin-top: 10upx;
		color: #333;
	
		.title,
		.time {
			display: flex;
			padding: 5upx 0;
		}
	
		.name {
			flex: 1;
			font-weight: 600;
			font-size: 30upx;
		}
	
		.amount {
			font-weight: 600;
			font-size: 30upx;
		}
	
		.date {
			flex: 1;
			color: #888;
			font-size: 28upx;
		}
	
		.status {
			border-radius: 500upx;
			background: red;
			padding: 0upx 16upx;
			color: #fff;
			font-size: 25upx;
			display: flex;
			align-items: center;
		}
	
		.SUCCESS {
			background: #44bf1a;
		}
	
		.PAY_WAIT {
			background: #969696;
		}
	
		.FAIL {
			background: #f92c2c;
		}
	}
	
	.load-more-box {
		height: 120upx;
	}
</style>
