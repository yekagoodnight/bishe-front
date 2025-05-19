<template>
	<div class="main-containers">
		<div class="body-containers">
			<navigation
				:menu-list="menuList"
				:icon-arr="iconArr"
				:base-url="baseUrl"
				:token="Token"
				:username="username"
				:headportrait="headportrait"
				@select="handleSelect"
				@go-menu="goMenu"
				@go-chat="goChat"
				@logout="logout"
				@to-login="toLogin"
			/>

			<div class="banner-preview" v-if="carouselChange()">
				<el-carousel trigger="click" indicator-position="inside" arrow="always" type="default" direction="horizontal" height="400px" :autoplay="true" :interval="3000" :loop="true">
					<el-carousel-item v-for="item in carouselList" :key="item.id">
						<el-image v-if="preHttp(item.value)" @click="carouselClick(item.url)" :src="item.value" fit="cover"></el-image>
						<el-image v-else @click="carouselClick(item.url)" :src="baseUrl + item.value" fit="cover"></el-image>
						<div class="banner-hidden">
						</div>
					</el-carousel-item>
				</el-carousel>
			</div>
			<!-- 首页内容间隔区域 -->
			<div class="home-spacer" v-if="carouselChange()"></div>
			<!-- 非首页顶部间距 -->
			<div class="other-page-spacer" v-if="!carouselChange()"></div>
			<router-view id="scrollView"></router-view>
			
			<div class="bottom-preview">
				<div class="footer"><div v-html="bottomContent"></div></div>
			</div>
		</div>
		
		<el-dialog title="智能AI" :visible.sync="chatFormVisible" width="60%" :before-close="chatClose">
			<div class="chat-content" id="chat-content">
				<div v-bind:key="item.id" v-for="item in chatList">
					<div v-if="item.addtime" style="width: 100%;text-align: center;font-size: 10px;color: #666;">{{timeFormat(item.addtime)}}</div>
					<div v-if="item.ask" class="right-content">
						<div style="display: flex;align-items: flex-start;">
							<el-alert v-if="item.type==1" class="text-content" :title="item.ask" :closable="false"
								type="warning"></el-alert>
							<el-image v-else-if="item.type==2" :src="baseUrl + item.ask" style="width: 150px;height: 150px;" fit="cover" :preview-src-list="[baseUrl + item.ask]"></el-image>
							<video v-else-if="item.type==3" :src="baseUrl + item.ask" style="width: 280px;" controls></video>
							<el-button v-else-if="item.type==4" type="primary" size="mini" @click="download(item.ask)">文件预览</el-button>
							<img :src="item.uimage?(baseUrl + item.uimage.split(',')[0]):require('@/assets/avator.png')" style="width: 30px;height: 30px;border-radius: 50%;margin: 0 0 0 5px;" alt="">
						</div>
					</div>
					<div v-else class="left-content">
						<div style="display: flex;align-items: flex-start;">
							<img :src="item.uimage?(baseUrl + item.uimage.split(',')[0]):require('@/assets/AI.png')" style="width: 30px;height: 30px;border-radius: 50%;margin: 0 5px 0 0;" alt="">
							<el-alert v-if="item.type==1" class="text-content" :title="item.reply" :closable="false"
								type="success"></el-alert>
							<el-image v-else-if="item.type==2" :src="baseUrl + item.reply" style="width: 150px;height: 150px;" fit="cover" :preview-src-list="[baseUrl + item.reply]"></el-image>
							<video v-else-if="item.type==3" :src="baseUrl + item.reply" style="width: 280px;" controls></video>
							<el-button v-else-if="item.type==4" type="primary" size="mini" @click="download(item.reply)">文件预览</el-button>
						</div>
					</div>
					<div class="clear-float"></div>
				</div>
			</div>
			<div v-if="aiLoading" v-loading="true" element-loading-background="rgba(255, 255, 255, 0.2)" style="text-align: center">
				AI正在解答您的问题，请稍后...
			</div>
			<div slot="footer" class="dialog-footer">
				<div v-if="askShow"
					style="padding-bottom: 10px;display: flex;align-items: center;justify-content: center;">
					<el-upload class="upload-demo" :action="uploadUrl" :on-success="uploadSuccess" accept=".jpg,.png"
						:show-file-list="false">
						<el-button size="mini" type="success">上传图片</el-button>
					</el-upload>
					<el-upload class="upload-demo" :action="uploadUrl" :on-success="uploadSuccess2" accept=".mp4"
						:show-file-list="false">
						<el-button size="mini" type="success" style="margin: 0 0 0 10px;">上传视频</el-button>
					</el-upload>
					<el-upload class="upload-demo" :action="uploadUrl" :on-success="uploadSuccess3"
						:show-file-list="false">
						<el-button size="mini" type="success" style="margin: 0 0 0 10px;">上传文件</el-button>
					</el-upload>
					<el-button size="mini" type="primary" style="margin: 0 0 0 10px;" @click="askChange">
						转{{askType==1?'人工服务':'智能回复'}}</el-button>
				</div>
				<div style="width: 100%;display: flex;align-items: center;justify-content: space-between;">
					<img style="width: 30px;cursor: pointer;" @click="askShow = !askShow" src="../assets/jiahao.png">
					<el-input @keydown.enter.native="addChat(null)" v-model="form.ask" placeholder="请输入内容" style="width: calc(100% - 110px);">
					</el-input>
					<el-button type="primary" @click="addChat(null)">发送</el-button>
					<div style="position: relative;" v-if="askType==2">
						<span @click="showEmoji=!showEmoji" class="icon iconfont icon-gerenzhongxin-zhihui" style="font-size: 30px;color: #666;cursor: pointer;"></span>
						<picker
							:include="['people', 'Smileys']"
							:showSearch="false"
							:showPreview="false"
							:showCategories="false"
							@select="addEmoji"
							v-if="showEmoji"
							:backgroundImageFn="((set,sheetSize)=>{
								return require('@/assets/32.png')
							})"
							style="position: absolute;bottom: 40px;left: -100px;"
						/>
					</div>
				</div>
			</div>
		</el-dialog>
	</div>
</template>

<script>
	import Vue from 'vue'
	import Swiper from "swiper";
	import axios from 'axios'
	import { Picker } from "emoji-mart-vue";
	import timeMethod from '@/common/timeMethod'
	import {
		WebsocketMixin
	} from '@/mixins/WebsocketMixin'
	import Navigation from '@/components/Navigation'
export default {
	components:{
		Picker,
		Navigation
	},
	mixins: [WebsocketMixin],
	data() {
		return {
			activeIndex: '0',
			baseUrl: '',
			carouselList: [],
			menuList: [],
			askType: 1, //1为智能回复 2为人工服务
			chatFormVisible: false,
			chatList: [],
			headers: {
				Token: localStorage.getItem('frontToken')
			},
			uploadUrl: this.$config.baseUrl + 'file/upload',
			askShow: false,
			aiLoading: false,
			showEmoji: false,
			form: {
				ask: '',
			},
			headportrait: localStorage.getItem('frontHeadportrait')?localStorage.getItem('frontHeadportrait'):'',
			Token: localStorage.getItem('frontToken'),
			username: localStorage.getItem('username'),
			notAdmin: localStorage.getItem('frontSessionTable')!='"users"',
			iconArr: [
				'el-icon-star-off',
				'el-icon-goods',
				'el-icon-warning',
				'el-icon-question',
				'el-icon-info',
				'el-icon-help',
				'el-icon-picture-outline-round',
				'el-icon-camera-solid',
				'el-icon-video-camera-solid',
				'el-icon-video-camera',
				'el-icon-bell',
				'el-icon-s-cooperation',
				'el-icon-s-order',
				'el-icon-s-platform',
				'el-icon-s-operation',
				'el-icon-s-promotion',
				'el-icon-s-release',
				'el-icon-s-ticket',
				'el-icon-s-management',
				'el-icon-s-open',
				'el-icon-s-shop',
				'el-icon-s-marketing',
				'el-icon-s-flag',
				'el-icon-s-comment',
				'el-icon-s-finance',
				'el-icon-s-claim',
				'el-icon-s-opportunity',
				'el-icon-s-data',
				'el-icon-s-check'
			],
			bottomContent: '',
			showType4: -1,
		}
	},
	async created() {
		this.baseUrl = this.$config.baseUrl;
		this.menuList = this.$config.indexNav;
		this.getCarousel();
		if(localStorage.getItem('frontToken') && localStorage.getItem('frontToken')!=null) {
			this.getSession()
		}
	},
	mounted() {
		this.activeIndex = localStorage.getItem('keyPath') || '0';



	},
	computed: {
		activeMenu() {
			const route = this.$route
			const {
				meta,
				path
			} = route
			// if st path, the sidebar will highlight the path you sete
			if (meta.activeMenu) {
				return meta.activeMenu
			}
			return path
		},
	},
	watch: {
		$route(newValue) {
			let that = this
			let url = window.location.href
			let arr = url.split('#')
			for (let x in this.menuList) {
				if (newValue.path == this.menuList[x].url) {
					this.activeIndex = x
				}
			}
			this.Token = localStorage.getItem('frontToken')
			if(arr[1]!='/index/home'){
				var element = document.getElementById('scrollView');
				var distance = element.offsetTop;
				window.scrollTo( 0, distance )
			}else{
				window.scrollTo( 0, 0 )
			}
		},
		headportrait(){
			this.$forceUpdate()
		},
	},
	methods: {
		preHttp(str) {
			return str && str.substr(0,4)=='http';
		},

		async getSession() {
			await this.$http.get(`${localStorage.getItem('UserTableName')}/session`, {emulateJSON: true}).then(async res => {
				if (res.data.code == 0) {
					localStorage.setItem('sessionForm', JSON.stringify(res.data.data))
					localStorage.setItem('frontUserid', res.data.data.id);
					if(res.data.data.vip) {
						localStorage.setItem('vip', res.data.data.vip);
					}
					
					let headportraitUrl = '';
					
					if(res.data.data.touxiang) {
						// 处理头像URL
						headportraitUrl = this.processAvatarUrl(res.data.data.touxiang);
						this.headportrait = headportraitUrl;
						localStorage.setItem('frontHeadportrait', headportraitUrl);
					} else if(res.data.data.headportrait) {
						// 处理头像URL
						headportraitUrl = this.processAvatarUrl(res.data.data.headportrait);
						this.headportrait = headportraitUrl;
						localStorage.setItem('frontHeadportrait', headportraitUrl);
					}
					
					console.log('用户头像URL:', headportraitUrl);
				}
			});
		},
		// 处理头像URL的统一函数
		processAvatarUrl(url) {
			if (!url) {
				return '';
			}
			
			// 多图只取第一个
			let avatarUrl = url.split(',')[0];
			
			// 特殊处理错误的头像路径
			if (avatarUrl.includes('1747475186509.jpg')) {
				return 'http://localhost:8080/springboot0aqexa96/upload/yonghu_touxiang1.jpg';
			}
			
			// 检查是否已经是完整URL
			if (avatarUrl.startsWith('http')) {
				return avatarUrl;
			}
			
			// 如果包含upload路径但不是完整URL
			if (avatarUrl.includes('/upload/')) {
				return 'http://localhost:8080/springboot0aqexa96' + avatarUrl;
			}
			
			// 默认情况，使用baseUrl拼接
			return this.baseUrl + avatarUrl;
		},
		handleSelect(keyPath) {
			if (keyPath) {
				localStorage.setItem('keyPath', keyPath)
			}
		},
		toLogin() {
		  this.$router.push('/login');
		},
		logout() {
			localStorage.clear();
			Vue.http.headers.common['Token'] = "";
			this.$router.push('/index/home');
			this.activeIndex = '0'
			localStorage.setItem('keyPath', this.activeIndex)
			this.Token = ''
			this.$forceUpdate()
			this.$message({
				message: '登出成功',
				type: 'success',
				duration: 1000,
			});
		},
		getCarousel() {
			this.$http.get('config/list', {params: { page: 1, limit: 3 }}).then(res => {
				if (res.data.code == 0) {
					this.carouselList = res.data.data.list;
				}
			});
		},
		// 轮播图跳转
		carouselClick(url) {
			if (url) {
				if (url.indexOf('https') != -1) {
					window.open(url)
				} else {
					this.$router.push(url)
				}
			}
		},
		carouselChange(){
			let url = window.location.href
			let arr = url.split('#')
			return arr[1]=='/index/home' // 只在系统首页显示轮播图
		},
		goBackend() {
			localStorage.setItem('Token', localStorage.getItem('frontToken'));
			localStorage.setItem('role', localStorage.getItem('frontRole'));
			localStorage.setItem('sessionTable', localStorage.getItem('frontSessionTable'));
			localStorage.setItem('headportrait', localStorage.getItem('frontHeadportrait'));
			localStorage.setItem('userid', localStorage.getItem('frontUserid'));
			window.location.href = `${this.$config.baseUrl}admin/dist/index.html`
			
		},
		formatMessages(messages) {
			let lastTime = null;
			messages.forEach((message, index) => {
				const currentTime = new Date(message.addtime).getTime();
				if (lastTime !== null) {
					const timeDiff = (currentTime - lastTime) / 1000 / 60; // 转换为分钟
					if (timeDiff < 3) {
						message.addtime = ''; // 如果小于3分钟，不显示时间
					}
				}
				lastTime = currentTime;
			});
			return messages;
		},
		timeFormat(time) {
			const Time = timeMethod.getTime(time).split("T");
			//当前消息日期属于周
			const week = timeMethod.getDateToWeek(time);
			//当前日期0时
			const nti = timeMethod.setTimeZero(timeMethod.getNowTime());
			//消息日期当天0时
			const mnti = timeMethod.setTimeZero(timeMethod.getTime(time));
			//计算日期差值
			const diffDate = timeMethod.calculateTime(nti, mnti);
			//本周一日期0时 （后面+1是去除当天时间）
			const fwnti = timeMethod.setTimeZero(timeMethod.countDateStr(-timeMethod.getDateToWeek(timeMethod
				.getNowTime()).weekID + 1));
			//计算周日期差值
			const diffWeek = timeMethod.calculateTime(mnti, fwnti);
		
			if (diffDate === 0) { //消息发送日期减去当天日期如果等于0则是当天时间
				return Time[1].slice(0, 5);
			} else if (diffDate < 172800000) { //当前日期减去消息发送日期小于2天（172800000ms）则是昨天-  一天最大差值前天凌晨00:00:00到今天晚上23:59:59
				return "昨天 " + Time[1].slice(0, 5);
			} else if (diffWeek >= 0) { //消息日期减去本周一日期大于0则是本周
				return week.weekName;
			} else { //其他时间则是日期
				return Time[0].slice(5, 10);
			}
		},
		addEmoji(e) {
			this.form.ask += e.native;
			this.showEmoji = false
		},
		getChatList() {
			this.$http.get('chat/list', {params: { userid: Number(localStorage.getItem('frontUserid')), sort: 'addtime', order: 'asc',limit: 1000 }}).then(res => {
				if (res.data.code == 0) {
					this.chatList = this.formatMessages(res.data.data.list);
					let div = document.getElementsByClassName('chat-content')[0]
					setTimeout(() => {
						if (div){
							div.scrollTop = div.scrollHeight
						}
					}, 0)
				}
			});
		},
		addChat(ask=null,type=1) {
			let params = JSON.parse(JSON.stringify(this.form))
			if(params.ask==''&&ask==null){
				this.$message.error('内容不能为空')
				return false
			}
			if(ask){
				params.ask = ask
			}
			params.type = type
			params.uimage = localStorage.getItem('frontHeadportrait')
			params.uname = localStorage.getItem('username')
			params.userid = Number(localStorage.getItem('frontUserid'))
			this.$http.post('chat/add', params).then(res => {
				if (res.data.code == 0) {
					this.getChatList();
					if (this.askType == 1 && ask == null) {
						let ask2 = JSON.parse(JSON.stringify(this.form.ask))
						this.getChathelper(ask2)
					}
					if(this.askType==2){
						this.websocketSend(ask?ask:params.ask)
					}
					this.form.ask = '';
				}
			});
		},
		chatClose() {
			if(this.askType==2){
				this.websocketOnclose();
			}
			this.chatFormVisible = false;
		},
		websocketOnmessage:function(e) {
			this.getChatList()
		},
		goChat() {
			if(!localStorage.getItem('frontToken')) {
				this.toLogin();
				return;
			}
			this.askType = 1
			this.saveChathelper('主人，我是您的智能助手小搏，请问有什么可以帮您！');
			this.getChatList();
			this.chatFormVisible = true;
		},
		uploadSuccess(res) {
			if (res.code == 0) {
				this.askShow = !this.askShow;
				this.addChat('upload/' + res.file,2)
			}
		},
		uploadSuccess2(res) {
			if (res.code == 0) {
				this.askShow = !this.askShow;
				this.addChat('upload/' + res.file,3)
			}
		},
		uploadSuccess3(res) {
			if (res.code == 0) {
				this.askShow = !this.askShow;
				this.addChat('upload/' + res.file,4)
			}
		},
		download(url){
			if(!url){
				return false
			}
			window.open((location.href.split(this.$config.name).length>1 ? location.href.split(this.$config.name)[0] + this.$config.name + '/' + url :this.$config.baseUrl + url))
		},
		getChathelper(ask) {
			this.aiLoading = true
			let div = document.getElementsByClassName('chat-content')[0]
			console.log(div)
			setTimeout(() => {
				if (div){
					div.scrollTop = div.scrollHeight
				}
			}, 100)
			this.$http.post('baidu/askai', {
				ask: `${ask}`,
			}).then(res => {
				if (res.data.code == 0) {
					this.aiLoading = false
					this.saveChathelper(res.data.data);
				}else {
					this.aiLoading = false
					this.saveChathelper('主人，AI还不够聪明，无法理解您的意思！')
				}
			});
		},
		saveChathelper(reply) {
			this.$http.post('chat/save', {
				reply: reply,
				userid: Number(localStorage.getItem('frontUserid')),
				type: 1
			}).then(res => {
				if (res.data.code == 0) {
					this.form.ask = '';
					this.getChatList();
				}
			});
		},
		askChange() {
			this.askShow = !this.askShow;
			this.askType = this.askType == 1 ? 2 : 1
			if(this.askType==1) {
				this.saveChathelper('主人，我是您的智能助手小搏，请问有什么可以帮您！');
				this.websocketOnclose();
			} else if(this.askType==2){
				this.saveChathelper('您好，在线客服很高兴为您服务！');
				this.initWebSocket(1)
			}
		},
		menuShowClick4(index){
			this.showType4 = index
		},
		goMenu(path) {
			this.$router.push(path);
			// 更新activeIndex，匹配对应的菜单项
			for (let i = 0; i < this.menuList.length; i++) {
				if (this.menuList[i].url === path) {
					this.activeIndex = i.toString();
					localStorage.setItem('keyPath', this.activeIndex);
					break;
				} else if (path.includes(this.menuList[i].url.replace('/index/', ''))) {
					// 处理从个人中心跳转的情况，通过部分URL匹配
					this.activeIndex = i.toString();
					localStorage.setItem('keyPath', this.activeIndex);
					break;
				}
			}
		},
	}
}
</script>

<style rel="stylesheet/css" lang="css" scoped>
	.main-containers {
		.body-containers {
			padding: 0;
			margin: 0;
			background: #F9F9F9;
			min-height: 100vh;
			position: relative;

			.menu-preview {
				padding: 10px 20px;
				background: rgba(27, 62, 144, 0.02);
				width: 100%;
				border: none;
				height: auto;
				box-shadow: 0 1px 6px rgba(27, 62, 144, 0.08);
				
				.el-scrollbar {
					height: 100%;
				}
				
				.el-menu-horizontal-demo {
					border: 0;
					padding: 0;
					margin: 0 0 0 auto;
					background: transparent;
					display: flex;
					width: 100%;
					position: relative;
					list-style: none;
					align-items: center;
					justify-content: space-between;

					/deep/ .el-menu-item {
						border-bottom: none !important;
					}

					.left-menu {
						display: flex;
						align-items: center;

						.system-title {
							font-size: 22px;
							font-weight: bold;
							color: #1B3E90;
							margin-right: 40px;
							cursor: pointer;
							text-shadow: 1px 1px 3px rgba(0,0,0,0.1);
							transition: all 0.3s ease;

							&:hover {
								transform: scale(1.02);
								text-shadow: 1px 1px 5px rgba(0,0,0,0.2);
							}
						}

						.menu-items {
							display: flex;
							align-items: center;
						}
					}

					.right-menu {
						display: flex;
						align-items: center;

						.user-info {
							display: flex;
							align-items: center;
							margin-right: 20px;

							.avatar {
								width: 36px;
								height: 36px;
								border-radius: 50%;
								margin-right: 10px;
								border: 2px solid rgba(27, 62, 144, 0.1);
								box-shadow: 0 2px 6px rgba(0,0,0,0.1);
								transition: all 0.3s ease;

								&:hover {
									transform: scale(1.05);
									border-color: rgba(27, 62, 144, 0.2);
									box-shadow: 0 3px 8px rgba(0,0,0,0.15);
								}
							}

							.username {
								color: #333;
								font-size: 14px;
								font-weight: 500;
								transition: all 0.3s ease;

								&:hover {
									color: #1B3E90;
								}
							}
						}

						.btn-login {
							border: 0;
							padding: 0 20px;
							margin: 0 10px;
							color: #fff;
							background: #1B3E90;
							font-size: 14px;
							line-height: 32px;
							height: 32px;
							border-radius: 16px;
							transition: all 0.3s ease;

							.icon {
								margin-right: 5px;
							}

							&:hover {
								background: #25489C;
								transform: translateY(-1px);
								box-shadow: 0 3px 8px rgba(27, 62, 144, 0.2);
							}
						}

						.btn-logout {
							border: 1px solid rgba(27, 62, 144, 0.15);
							padding: 0 20px;
							color: #1B3E90;
							background: rgba(27, 62, 144, 0.05);
							font-size: 14px;
							line-height: 32px;
							height: 32px;
							border-radius: 16px;
							transition: all 0.3s ease;

							.icon {
								margin-right: 5px;
								transition: all 0.3s ease;
							}

							&:hover {
								color: #fff;
								background: #1B3E90;
								border-color: #1B3E90;
								transform: translateY(-1px);
								box-shadow: 0 3px 8px rgba(27, 62, 144, 0.2);

								.icon {
									transform: rotate(90deg);
								}
							}
						}
					}

					.el-menu-item.home, .el-menu-item.item, .el-menu-item.service {
						cursor: pointer;
						padding: 0 15px;
						margin: 0 5px;
						color: #333;
						white-space: nowrap;
						font-weight: 500;
						display: flex;
						font-size: 14px;
						line-height: 40px;
						height: 40px;
						align-items: center;
						border-radius: 20px;
						transition: all 0.3s ease;

						.icon, .icons {
							margin-right: 5px;
							font-size: 16px;
							transition: all 0.3s ease;
						}

						&:hover {
							color: #1B3E90;
							background: rgba(27, 62, 144, 0.05);

							.icon, .icons {
								transform: scale(1.1);
							}
						}

						&.is-active {
							color: #1B3E90;
							background: rgba(27, 62, 144, 0.08);
							font-weight: bold;
						}
					}
				}
			}
			.banner-preview {
				width: 100%;
				height: auto;
				opacity: 1;
				transition: opacity 0.5s ease, height 0.5s ease;
				overflow: hidden;
				
				.el-carousel {
					margin: 0 auto;
					width: 100%;
					/deep/ .el-carousel__item {
						border-radius: 0;
						width: 100%;
						height: 100%;
						@keyframes wave1 {from { left: -236px } to { left: -1233px }}
						@keyframes wave2 {from { left: 0 } to { left: -1009px }}
						.el-image {
							object-fit: cover;
							width: 100%;
							height: 100%;
							transition: transform 0.7s ease;
							&:hover {
								transform: scale(1.02);
							}
						}
						
						.banner-hidden {
							position: absolute;
							top: 0;
							left: 0;
							right: 0;
							bottom: 0;
							background: linear-gradient(to bottom, rgba(0,0,0,0.05) 0%, rgba(0,0,0,0.2) 100%);
							z-index: 1;
						}
					}
					/deep/ .el-carousel__container .el-carousel__arrow--left {
						width: 46px;
						font-size: 14px;
						height: 46px;
						background: rgba(0, 0, 0, 0.3);
						border-radius: 50%;
						transition: all 0.3s ease;
						z-index: 10;
					}
					/deep/ .el-carousel__container .el-carousel__arrow--left:hover {
						background: #1B3E90;
						transform: translateX(-3px);
					}
					/deep/ .el-carousel__container .el-carousel__arrow--right {
						width: 46px;
						font-size: 14px;
						height: 46px;
						background: rgba(0, 0, 0, 0.3);
						border-radius: 50%;
						transition: all 0.3s ease;
						z-index: 10;
					}
					/deep/ .el-carousel__container .el-carousel__arrow--right:hover {
						background: #1B3E90;
						transform: translateX(3px);
					}
					/deep/ .el-carousel__indicators {
						padding: 0;
						margin: 0;
						z-index: 2;
						position: absolute;
						list-style: none;
						bottom: 20px;
						li {
							padding: 0;
							margin: 0 6px;
							background: #fff;
							display: inline-block;
							width: 10px;
							border-radius: 10px;
							opacity: 0.4;
							transition: all 0.3s ease;
							height: 10px;
						}
						li:hover {
							padding: 0;
							margin: 0 6px;
							background: #fff;
							display: inline-block;
							width: 30px;
							opacity: 0.7;
							height: 10px;
						}
						li.is-active {
							padding: 0;
							margin: 0 6px;
							background: #fff;
							display: inline-block;
							width: 30px;
							opacity: 1;
							height: 10px;
						}
					}
					/deep/ .el-carousel__indicator button {
						width: 0;
						height: 0;
						display: none;
					}
				}
			}
			.home-spacer {
				height: 30px;
				background: rgba(255,255,255,0.5);
				width: 100%;
				position: relative;
				
				&:after {
					content: "";
					position: absolute;
					bottom: 0;
					left: 16%;
					right: 16%;
					height: 1px;
					background: rgba(0,0,0,0.05);
				}
			}
			.other-page-spacer {
				height: 40px;
				background: linear-gradient(to bottom, #fff 0%, #f9f9f9 100%);
				width: 100%;
				position: relative;
				box-shadow: inset 0 -1px 3px rgba(0,0,0,0.03);
				
				&:after {
					content: "";
					position: absolute;
					bottom: 0;
					left: 16%;
					right: 16%;
					height: 1px;
					background: rgba(0,0,0,0.05);
				}
			}
			.bottom-preview {
				width: 100%;
				height: auto;
				margin-top: 30px;
				.footer {
					padding: 30px;
					overflow: hidden;
					background: linear-gradient(135deg, #152b5e 0%, #1B3E90 100%);
					width: 100%;
					height: auto;
					color: rgba(255,255,255,0.9);
					font-size: 14px;
					text-align: center;
					box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
					
					a {
						color: #fff;
						text-decoration: none;
						margin: 0 10px;
						transition: all 0.3s ease;
						
						&:hover {
							opacity: 0.8;
							text-decoration: underline;
						}
					}
				}
			}
		}
	}

	/* 面包屑导航样式 */
	.breadcrumb-preview {
		padding: 0 16%;
		margin: 0;
		background: transparent;
		width: 100%;
		box-shadow: none;
		position: relative;
	}
	
	/* 精确的选择器匹配您的DOM路径 */
	#scrollView > div.breadcrumb-preview > div > div .el-breadcrumb__inner.is-link {
		font-size: 18px;
	}
	
	.breadcrumb-preview:before {
		content: "";
		position: absolute;
		left: 0;
		top: 0;
		bottom: 0;
		width: 6px;
		background: linear-gradient(to bottom, #1B3E90, #3561c5);
	}
	
	.breadcrumb-preview .breadcrumb-container {
		padding: 20px 0;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb {
		font-size: 18px;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__inner.is-link {
		font-size: 18px;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__item .el-breadcrumb__inner {
		color: #444;
		font-weight: 500;
		transition: all 0.3s;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__item .el-breadcrumb__inner:hover {
		color: #1B3E90;
		transform: scale(1.05);
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__item .el-breadcrumb__inner a {
		font-weight: 500;
		color: #444;
		transition: all 0.3s;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__item .el-breadcrumb__inner a:hover {
		color: #1B3E90;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__item:last-child .el-breadcrumb__inner {
		color: #1B3E90;
		font-weight: 700;
		font-size: 19px;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .el-breadcrumb__separator {
		color: #666;
		margin: 0 12px;
		font-size: 20px;
		font-weight: bold;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb .item1 i {
		margin-right: 8px;
		font-size: 19px;
		color: #1B3E90;
	}

	.chat-content {
		padding: 15px;
		width: 100%;
		margin-bottom: 15px;
		max-height: 600px;
		height: 600px;
		overflow-y: auto;
		border: 1px solid #eee;
		background: #f9f9f9;
		border-radius: 8px;
		box-shadow: inset 0 2px 10px rgba(0,0,0,0.03);
		scrollbar-width: thin;
		scrollbar-color: #ccc transparent;

		&::-webkit-scrollbar {
			width: 6px;
		}
		
		&::-webkit-scrollbar-track {
			background: transparent;
		}
		
		&::-webkit-scrollbar-thumb {
			background-color: #ccc;
			border-radius: 6px;
			border: 2px solid transparent;
		}

		.left-content {
			float: left;
			margin-bottom: 15px;
			padding: 0;
			max-width: 80%;
			
			.text-content {
				margin: 0;
				border-radius: 18px 18px 18px 0;
				box-shadow: 0 2px 6px rgba(0,0,0,0.05);
				
				/deep/ .el-alert__title {
					font-size: 14px;
					line-height: 1.5;
				}
			}
		}

		.right-content {
			float: right;
			margin-bottom: 15px;
			padding: 0;
			max-width: 80%;
			
			.text-content {
				margin: 0;
				border-radius: 18px 18px 0 18px;
				box-shadow: 0 2px 6px rgba(0,0,0,0.05);
				
				/deep/ .el-alert__title {
					font-size: 14px;
					line-height: 1.5;
				}
			}
		}
	}

	.clear-float {
		clear: both;
	}

	.el-dialog {
		border-radius: 8px;
		overflow: hidden;
		box-shadow: 0 10px 30px rgba(0,0,0,0.15);
		
		/deep/ .el-dialog__header {
			background: #f8f8f8;
			padding: 15px 20px;
			border-bottom: 1px solid #eee;
			
			.el-dialog__title {
				font-weight: 600;
				color: #1B3E90;
				font-size: 18px;
			}
		}
		
		/deep/ .el-dialog__body {
			padding: 20px;
		}
		
		/deep/ .el-dialog__footer {
			border-top: 1px solid #eee;
			padding: 15px 20px;
		}
	}

	.dialog-footer {
		.el-button {
			border-radius: 20px;
			transition: all 0.3s ease;
			
			&:hover {
				transform: translateY(-2px);
				box-shadow: 0 2px 5px rgba(0,0,0,0.1);
			}
		}
		
		.el-input {
			/deep/ .el-input__inner {
				border-radius: 20px;
				height: 40px;
			}
		}
	}

	.emoji-mart[data-v-7bc71df8] {
		font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", sans-serif;
		display: -ms-flexbox;
		display: flex;
		-ms-flex-direction: column;
		flex-direction: column;
		height: 420px;
		color: #333 !important;
		border: 1px solid #d9d9d9;
		border-radius: 8px;
		background: #fff;
		box-shadow: 0 5px 15px rgba(0,0,0,0.1);
	}
</style>
