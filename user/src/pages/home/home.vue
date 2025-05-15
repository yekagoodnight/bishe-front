<template>
	<div class="home-preview">
		<!-- 新闻资讯 -->
		<div id="animate_newsnews" class="news animate__animated">
			<div style="position:relative;width:300px;margin-bottom:20px;">
				<div class="news_title_box" style="width:300px;line-height:60px;">
					<span class="news_title" style="font-size:24px;">新闻资讯</span>
					<span class="news_subhead">{{'news'.toUpperCase()}}</span>
				</div>
				<div style="content:'';position:absolute;height:2px;width:calc(100vw - 1220px);background:linear-gradient(90deg, #1B3E90 0%, transparent 100%);left:320px;top:30px;transform:translateY(-50%)"></div>
			</div>
			<!-- 样式四 -->
			<div v-if="newsList.length" class="list list4 index-pv1">
				<div @click="toDetail('newsDetail', newsList[0])" v-if="newsList.length>0" class="list-item1 animation-box">
					<div class="name line1">{{newsList[0].title}}</div>
					<div class="desc line3">{{newsList[0].introduction}}</div>
					<div class="item-footer">
						<div class="time_item">
							<span class="icon iconfont icon-shijian21"></span>
							<span class="label">发布时间：</span>
							<span class="text">{{newsList[0].addtime.split(' ')[0]}}</span>
						</div>
						<div class="publisher_item">
							<span class="icon iconfont icon-geren16"></span>
							<span class="label">发布人：</span>
							<span class="text">{{newsList[0].name}}</span>
						</div>
						<div class="like_item">
							<span class="icon iconfont icon-zan10"></span>
							<span class="label">点赞数：</span>
							<span class="text">{{newsList[0].thumbsupnum}}</span>
						</div>
						<div class="collect_item">
							<span class="icon iconfont icon-shoucang10"></span>
							<span class="label">收藏量：</span>
							<span class="text">{{newsList[0].storeupnum}}</span>
						</div>
						<div class="view_item">
							<span class="icon iconfont icon-chakan9"></span>
							<span class="label">点击量：</span>
							<span class="text">{{newsList[0].clicknum}}</span>
						</div>
					</div>
				</div>
				<div @click="toDetail('newsDetail', newsList[0])" v-if="newsList.length>0" class="list-item2 animation-box">
					<img :src="baseUrl + newsList[0].picture" alt="">
				</div>
				<div @click="toDetail('newsDetail', newsList[1])" v-if="newsList.length>1" class="list-item3 animation-box">
					<div class="name line1">{{newsList[1].title}}</div>
					<div class="desc line3">{{newsList[1].introduction}}</div>
					<div class="item-footer">
						<div class="time_item">
							<span class="icon iconfont icon-shijian21"></span>
							<span class="label">发布时间：</span>
							<span class="text">{{newsList[1].addtime.split(' ')[0]}}</span>
						</div>
						<div class="publisher_item">
							<span class="icon iconfont icon-geren16"></span>
							<span class="label">发布人：</span>
							<span class="text">{{newsList[1].name}}</span>
						</div>
						<div class="like_item">
							<span class="icon iconfont icon-zan10"></span>
							<span class="label">点赞数：</span>
							<span class="text">{{newsList[1].thumbsupnum}}</span>
						</div>
						<div class="collect_item">
							<span class="icon iconfont icon-shoucang10"></span>
							<span class="label">收藏量：</span>
							<span class="text">{{newsList[1].storeupnum}}</span>
						</div>
						<div class="view_item">
							<span class="icon iconfont icon-chakan9"></span>
							<span class="label">点击量：</span>
							<span class="text">{{newsList[1].clicknum}}</span>
						</div>
					</div>
				</div>
				<div @click="toDetail('newsDetail', newsList[1])" v-if="newsList.length>1" class="list-item4 animation-box">
					<img :src="baseUrl + newsList[1].picture" alt="">
				</div>
				<div @click="toDetail('newsDetail', newsList[2])" v-if="newsList.length>2" class="list-item5 animation-box">
					<div class="name line1">{{newsList[2].title}}</div>
					<div class="desc line3">{{newsList[2].introduction}}</div>
					<div class="item-footer">
						<div class="time_item">
							<span class="icon iconfont icon-shijian21"></span>
							<span class="label">发布时间：</span>
							<span class="text">{{newsList[2].addtime.split(' ')[0]}}</span>
						</div>
						<div class="publisher_item">
							<span class="icon iconfont icon-geren16"></span>
							<span class="label">发布人：</span>
							<span class="text">{{newsList[2].name}}</span>
						</div>
						<div class="like_item">
							<span class="icon iconfont icon-zan10"></span>
							<span class="label">点赞数：</span>
							<span class="text">{{newsList[2].thumbsupnum}}</span>
						</div>
						<div class="collect_item">
							<span class="icon iconfont icon-shoucang10"></span>
							<span class="label">收藏量：</span>
							<span class="text">{{newsList[2].storeupnum}}</span>
						</div>
						<div class="view_item">
							<span class="icon iconfont icon-chakan9"></span>
							<span class="label">点击量：</span>
							<span class="text">{{newsList[2].clicknum}}</span>
						</div>
					</div>
				</div>
				<div @click="toDetail('newsDetail', newsList[2])" v-if="newsList.length>2" class="list-item6 animation-box">
					<img :src="baseUrl + newsList[2].picture" alt="">
				</div>
			</div>
			<div class="moreBtn" @click="moreBtn('news')">
				<span class="text">查看更多</span>
				<i class="icon iconfont icon-gengduo1"></i>
			</div>
		</div>
		<!-- 新闻资讯 -->
		<!-- 商品推荐 -->
		<div id="animate_recommendqichechuzu" class="recommend animate__animated">
			<div style="position:relative;width:300px;margin-bottom:25px;">
				<div class="recommend_title_box" style="width:300px;line-height:60px;">
					<span class="recommend_title" style="font-size:24px;">汽车租赁推荐</span>
					<span class="recommend_subhead">{{'qichechuzu'.toUpperCase()}} RECOMMEND</span>
				</div>
				<div style="content:'';position:absolute;height:2px;width:calc(100vw - 1220px);background:linear-gradient(90deg, #1B3E90 0%, transparent 100%);left:320px;top:30px;transform:translateY(-50%)"></div>
			</div>
			<div class="list list1 index-pv1">
				<div v-for="(item, index) in qichechuzuRecommend" :key="index" @click.stop="toDetail('qichechuzuDetail', item)" class="list-item animation-box">
					<div class="image-container">
						<img class="image" @click.stop="imgPreView(item.qichetupian)" v-if="item.qichetupian && item.qichetupian.substr(0,4)=='http'&& item.qichetupian.split(',w').length>1" :src="item.qichetupian" />
						<img class="image" @click.stop="imgPreView(item.qichetupian.split(',')[0])" v-else-if="item.qichetupian && item.qichetupian.substr(0,4)=='http'" :src="item.qichetupian.split(',')[0]" />
						<img class="image" @click.stop="imgPreView(baseUrl + (item.qichetupian?item.qichetupian.split(',')[0]:''))" v-else :src="baseUrl + (item.qichetupian?item.qichetupian.split(',')[0]:'')" />
						<div class="overlay">
							<i class="icon iconfont icon-sousuo"></i>
						</div>
					</div>
					<div class="content">
						<div class="name">{{item.zulinbiaoti}}</div>
						<div class="meta-info">
							<div class="time_item">
								<span class="icon iconfont icon-shijian21"></span>
								<span class="text">{{item.addtime.split(' ')[0]}}</span>
							</div>
							<div class="publisher_item">
								<span class="icon iconfont icon-geren16"></span>
								<span class="text">{{item.cheshangzhanghao}}</span>
							</div>
						</div>
						<div class="stats">
							<div class="collect_item">
								<span class="icon iconfont icon-shoucang10"></span>
								<span class="text">{{item.storeupnum}}</span>
							</div>
							<div class="view_item" v-if="item.clicknum">
								<span class="icon iconfont icon-chakan9"></span>
								<span class="text">{{item.clicknum}}</span>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div class="moreBtn" @click="moreBtn('qichechuzu')">
				<span class="text">查看更多</span>
				<i class="icon iconfont icon-gengduo1"></i>
			</div>
		</div>
		<!-- 商品推荐 -->
		<el-dialog title="预览图" :visible.sync="previewVisible" width="50%">
			<img :src="previewImg" alt="" style="width: 100%;">
		</el-dialog>
	</div>
</template>

<script>
import 'animate.css'
import Swiper from "swiper";

	export default {
		//数据集合
		data() {
			return {
				baseUrl: '',
				newsList: [],
				qichechuzuRecommend: [],
				previewImg: '',
				previewVisible: false,
				recommendListSwiper18qichechuzu: null,
				recommendIndex18qichechuzu: 0,
				recommendList18qichechuzu: [],
				recommendColumn18qichechuzu: '',
			}
		},
		created() {
			this.baseUrl = this.$config.baseUrl;
			this.getNewsList();
			this.getQichechuzuList();
		},
		mounted() {
			window.addEventListener('scroll', this.handleScroll)
			setTimeout(()=>{
				this.handleScroll()
			},100)
			
			this.swiperChanges()
		},
		beforeDestroy() {
			window.removeEventListener('scroll', this.handleScroll)
		},
		//方法集合
		methods: {
			swiperChanges() {
				if (this['recommendListSwiper18qichechuzu']) this['recommendListSwiper18qichechuzu'].destroy()
				setTimeout(()=>{
					this['recommendListSwiper18qichechuzu'] = new Swiper(".recommend-list-18qichechuzu", {"navigation":{"nextEl":".swiper-button-next","prevEl":".swiper-button-prev"},"pagination":{"el":".swiper-pagination","clickable":true},"loop":false,"coverflowEffect":{"rotate":10,"stretch":10,"depth":60,"modifier":3,"slideShadows":false},"centeredSlides":true,"slidesPerView":1,"spaceBetween":0,"autoplay":{"delay":3000,"disableOnInteraction":false}})
				},750)
			},
			async getQichechuzuList() {
				let params = {
					page: 1,
					limit: 8,
					sort: 'id',
					order: 'desc'
				};
				
				this.$http.get('qichechuzu/list', {params: params}).then(res => {
					if (res.data.code == 0) {
						this.qichechuzuRecommend = res.data.data.list;
					}
				});
			},
			imgPreView(url){
				this.previewImg = url
				this.previewVisible = true
			},
			handleScroll() {
				let arr = [
					{id:'about',css:'animate__'},
					{id:'system',css:'animate__'},
					{id:'animate_recommendqichechuzu',css:'animate__'},
					{id:'animate_newsnews',css:'animate__'},
				]
			
				for (let i in arr) {
					let doc = document.getElementById(arr[i].id)
					if (doc) {
						let top = doc.offsetTop
						let win_top = window.innerHeight + window.pageYOffset
						// console.log(top,win_top)
						if (win_top > top && doc.classList.value.indexOf(arr[i].css) < 0) {
							// console.log(doc)
							doc.classList.add(arr[i].css)
						}
					}
				}
			},
			preHttp(str) {
				return str && str.substr(0,4)=='http';
			},
			getNewsList() {
				let data = {
					page: 1,
					limit: 6,
					sort: 'addtime',
					order: 'desc'
				}
				this.$http.get('news/list', {params: data}).then(res => {
					if (res.data.code == 0) {
						this.newsList = res.data.data.list;
					
					}
				});
			},
			toDetail(path, item) {
				this.$router.push({path: '/index/' + path, query: {id: item.id}});
			},
			moreBtn(path) {
				this.$router.push({path: '/index/' + path});
			}
		}
	}
</script>

<style rel="stylesheet/css" lang="css" scoped>
	.home-preview {
		margin: 0px auto;
		flex-direction: column;
		display: flex;
		width: 100%;
		.news {
			padding: 30px 16%;
			margin: 0;
			background: #fff;
			width: 100%;
			position: relative;
			order: 5;
			.news_title_box {
				margin: 0 0 20px 0;
				background: linear-gradient(135deg, #f5f7ff 0%, #e8ecf8 100%);
				width: 400px;
				line-height: 80px;
				text-align: center;
				position: relative;
				border-radius: 8px;
				box-shadow: 0 4px 12px rgba(0,0,0,0.1);
				&:after {
					content: "";
					position: absolute;
					height: 2px;
					width: calc(100% - 400px);
					background: linear-gradient(90deg, #1B3E90 0%, transparent 100%);
					right: -100px;
					top: 50%;
					transform: translateY(-50%);
				}
				.news_title {
					color: #1B3E90;
					width: 100%;
					font-size: 30px;
					text-align: center;
					text-shadow: none;
				}
				.news_subhead {
					margin: 0 0 10px;
					color: #999;
					display: none;
					width: 100%;
					font-size: 20px;
					line-height: 1.5;
					text-align: center;
				}
			}
			.index-pv1 .animation-box:hover {
				transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 2px, 0px);
				-webkit-perspective: 1000px;
				perspective: 1000px;
				transition: 0.3s;
				z-index: 1;
				box-shadow: 0 5px 15px rgba(0,0,0,0.1);
			}
			.index-pv1 .animation-box img:hover {
				transform: rotate(0deg) scale(1.05) skew(1deg, 1deg) translate3d(0px, 0px, 0px);
				-webkit-perspective: 1000px;
				perspective: 1000px;
				transition: 0.3s;
			}
			.list4 {
				padding: 10px 0;
				background: #fff;
				display: flex;
				width: 100%;
				justify-content: center;
				flex-wrap: wrap;
				height: auto;
				gap: 15px;
				.list-item1, .list-item3, .list-item5 {
					border-radius: 10px;
					padding: 20px;
					clip-path: polygon(0% 0,35% 0,35% 5%,65% 5%,65% 0%, 100% 0, 100% 100%, 0% 100%);
					background: linear-gradient(135deg, #ECE1C3, #F8F4E8);
					display: flex;
					width: calc(100% / 3 - 10px);
					justify-content: flex-start;
					align-items: flex-start;
					flex-wrap: wrap;
					height: 260px;
					box-shadow: 0 3px 10px rgba(0,0,0,0.05);
					transition: all 0.3s ease;
					position: relative;
					overflow: hidden;
					.name {
						margin: 0 0 10px;
						overflow: hidden;
						color: #333;
						white-space: nowrap;
						width: 100%;
						font-size: 18px;
						font-weight: bold;
						text-overflow: ellipsis;
						text-align: left;
						position: relative;
						padding-bottom: 10px;
						&:after {
							content: "";
							position: absolute;
							bottom: 0;
							left: 0;
							width: 50px;
							height: 3px;
							background: #23469A;
						}
					}
					.desc {
						padding: 0;
						margin: 0 0 10px;
						color: #666;
						width: 100%;
						font-size: 15px;
						line-height: 1.5;
						max-height: 67.5px;
						overflow: hidden;
						display: -webkit-box;
						-webkit-line-clamp: 3;
						-webkit-box-orient: vertical;
					}
					.item-footer {
						position: absolute;
						bottom: 0;
						left: 0;
						right: 0;
						background: rgba(255,255,255,0.9);
						padding: 10px 20px;
						display: flex;
						flex-wrap: wrap;
						.time_item, .publisher_item, .like_item, .collect_item, .view_item {
							padding: 0 15px 0 0;
							display: flex;
							align-items: center;
							.icon {
								margin: 0 5px 0 0;
								font-size: 14px;
							}
							.label {
								display: none;
							}
							.text {
								font-size: 12px;
							}
						}
						.time_item .icon, .time_item .text {
							color: #E0C936;
						}
						.publisher_item .icon, .publisher_item .text {
							color: #43BAB1;
						}
						.like_item .icon, .like_item .text {
							color: #23469A;
						}
						.collect_item .icon, .collect_item .text {
							color: #AD8B44;
						}
						.view_item .icon, .view_item .text {
							color: #A76850;
						}
					}
				}
				.list-item2, .list-item4, .list-item6 {
					background: #f5f5f5;
					width: calc(100% / 3 - 10px);
					height: 260px;
					border-radius: 10px;
					overflow: hidden;
					box-shadow: 0 3px 10px rgba(0,0,0,0.05);
					img {
						border-radius: 0;
						object-fit: cover;
						width: 100%;
						height: 100%;
						transition: all 0.3s ease;
					}
					&:hover img {
						transform: scale(1.05);
					}
				}
			}
			.moreBtn {
				border: 0;
				border-radius: 5px;
				padding: 0 20px;
				margin: 20px auto 10px;
				top: 45px;
				background: #23469A;
				display: block;
				width: auto;
				line-height: 36px;
				position: absolute;
				right: 16%;
				text-align: center;
				transition: all 0.3s ease;
				&:hover {
					background: #1A347B;
					transform: translateY(-2px);
					box-shadow: 0 5px 15px rgba(0,0,0,0.1);
				}
				.text {
					color: #fff;
					font-size: 14px;
				}
				.icon {
					color: #fff;
					display: none;
					font-size: 14px;
				}
			}
		}
		.recommend {
			padding: 30px 16%;
			margin: 0;
			background: #fff;
			width: 100%;
			position: relative;
			order: 3;
			.recommend_title_box {
				margin: 0 0 25px 0;
				background: linear-gradient(135deg, #f5f7ff 0%, #e8ecf8 100%);
				width: 400px;
				line-height: 80px;
				text-align: center;
				position: relative;
				border-radius: 8px;
				box-shadow: 0 4px 12px rgba(0,0,0,0.1);
				&:after {
					content: "";
					position: absolute;
					height: 2px;
					width: calc(100% - 400px);
					background: linear-gradient(90deg, #1B3E90 0%, transparent 100%);
					right: -100px;
					top: 50%;
					transform: translateY(-50%);
				}
				.recommend_title {
					color: #1B3E90;
					width: 100%;
					font-size: 30px;
					text-align: center;
					text-shadow: none;
				}
				.recommend_subhead {
					margin: 0 0 10px;
					color: #999;
					display: none;
					width: 100%;
					font-size: 20px;
					line-height: 1.5;
					text-align: center;
				}
			}
			.index-pv1 .animation-box {
				transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
				z-index: initial;
				transition: all 0.4s ease;
			}
			
			.index-pv1 .animation-box:hover {
				transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, -10px, 0px);
				-webkit-perspective: 1000px;
				perspective: 1000px;
				z-index: 1;
				box-shadow: 0 15px 30px rgba(0,0,0,0.1);
			}
			
			.list1 {
				display: flex;
				flex-wrap: wrap;
				justify-content: space-between;
				margin-top: 20px;
				
				.list-item {
					width: 24%;
					margin-bottom: 30px;
					padding: 0;
					border-radius: 10px;
					background: #fff;
					transition: all 0.4s ease;
					cursor: pointer;
					box-shadow: 0 5px 15px rgba(0,0,0,0.05);
					overflow: hidden;
					position: relative;
					
					.image-container {
						width: 100%;
						height: 200px;
						position: relative;
						overflow: hidden;
						
						.image {
							width: 100%;
							height: 100%;
							object-fit: cover;
							transition: all 0.5s ease;
						}
						
						.overlay {
							position: absolute;
							top: 0;
							left: 0;
							width: 100%;
							height: 100%;
							background: rgba(35, 70, 154, 0.3);
							display: flex;
							align-items: center;
							justify-content: center;
							opacity: 0;
							transition: all 0.3s ease;
							
							.icon {
								color: #fff;
								font-size: 30px;
								transform: scale(0.5);
								transition: all 0.3s ease;
							}
						}
						
						&:hover {
							.image {
								transform: scale(1.1);
							}
							
							.overlay {
								opacity: 1;
								
								.icon {
									transform: scale(1);
								}
							}
						}
					}
					
					.content {
						padding: 15px;
						
						.name {
							font-size: 16px;
							font-weight: 600;
							margin-bottom: 10px;
							overflow: hidden;
							text-overflow: ellipsis;
							white-space: nowrap;
							color: #23469A;
							position: relative;
							padding-bottom: 10px;
							
							&:after {
								content: "";
								position: absolute;
								bottom: 0;
								left: 0;
								width: 40px;
								height: 2px;
								background: #23469A;
							}
						}
						
						.meta-info {
							display: flex;
							margin-bottom: 8px;
							
							.time_item, .publisher_item {
								display: flex;
								align-items: center;
								margin-right: 15px;
								
								.icon {
									color: #777;
									margin-right: 5px;
									font-size: 14px;
								}
								
								.label {
									display: none;
								}
								
								.text {
									color: #777;
									font-size: 12px;
									overflow: hidden;
									text-overflow: ellipsis;
									white-space: nowrap;
									max-width: 80px;
								}
							}
						}
						
						.stats {
							display: flex;
							border-top: 1px solid #f0f0f0;
							padding-top: 8px;
							
							.collect_item, .view_item {
								display: flex;
								align-items: center;
								margin-right: 15px;
								
								.icon {
									color: #AD8B44;
									margin-right: 5px;
									font-size: 14px;
								}
								
								.label {
									display: none;
								}
								
								.text {
									color: #AD8B44;
									font-size: 12px;
								}
							}
							
							.view_item .icon, .view_item .text {
								color: #A76850;
							}
						}
					}
					
					&:hover {
						background: #f9f9f9;
					}
				}
			}
			
			.moreBtn {
				border: 0;
				border-radius: 5px;
				padding: 0 20px;
				margin: 10px auto;
				top: 45px;
				background: #23469A;
				display: block;
				width: auto;
				line-height: 36px;
				position: absolute;
				right: 16%;
				text-align: center;
				transition: all 0.3s ease;
				box-shadow: 0 3px 10px rgba(0,0,0,0.1);
				
				&:hover {
					background: #1B3678;
					transform: translateY(-2px);
					box-shadow: 0 5px 15px rgba(0,0,0,0.2);
				}
				
				.text {
					color: #fff;
					font-size: 14px;
				}
				
				.icon {
					color: #fff;
					margin-left: 5px;
					display: inline-block;
					font-size: 14px;
				}
			}
		}
	}
	.section-title {
		margin: 0;
		color: #1B3E90;
		background: linear-gradient(135deg, #f5f7ff 0%, #e8ecf8 100%);
		width: 400px;
		font-size: 30px;
		line-height: 80px;
		text-align: center;
		border-radius: 8px;
		box-shadow: 0 4px 12px rgba(0,0,0,0.1);
		position: relative;
		&:after {
			content: "";
			position: absolute;
			left: 420px;
			top: 50%;
			height: 2px;
			width: calc(100vw - 820px);
			background: linear-gradient(90deg, #1B3E90 0%, transparent 100%);
			transform: translateY(-50%);
		}
	}
</style>
