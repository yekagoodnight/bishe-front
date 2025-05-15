<template>
	<div class="navbar">
		<div class="title">
			<span class="title-name">{{this.$project.projectName}}</span>
		</div>
		<el-dropdown class="dropdown-box" @command="handleCommand" trigger="click">
			<div class="el-dropdown-link">
				<el-image v-if="user" :src="avatar?this.$base.url + avatar : require('@/assets/img/avator.png')" fit="contain"></el-image>
				<span class="label">欢迎您，</span>
				<span class="nickname">{{this.$storage.get('adminName')}}</span>
				<span class="icon iconfont icon-xiala"></span>
			</div>
			<el-dropdown-menu class="top-el-dropdown-menu" slot="dropdown">
				<el-dropdown-item class="item1" :command="''">
					<span class="icon iconfont icon-home19"></span>
					首页
				</el-dropdown-item>
				<el-dropdown-item class="item2" :command="'center'">
					<span class="icon iconfont icon-touxiang03"></span>
					个人中心
				</el-dropdown-item>
				<el-dropdown-item v-if="this.$storage.get('role')!='管理员'" class="item3" :command="'front'">
					<span class="icon iconfont icon-fanhui12"></span>
					退出到前台
				</el-dropdown-item>
				<el-dropdown-item v-if="this.$storage.get('role')=='管理员'" class="item3" :command="'backUp'">
					<span class="icon iconfont icon-fanhui12"></span>
					数据备份
				</el-dropdown-item>
				<el-dropdown-item class="item4" :command="'logout'">
					<span class="icon iconfont icon-fanhui13"></span>
					退出登录
				</el-dropdown-item>
			</el-dropdown-menu>
		</el-dropdown>
	</div>
</template>

<script>
	import {
		Loading
	} from 'element-ui';
	import axios from 'axios';
	export default {
		data() {
			return {
				dialogVisible: false,
				ruleForm: {},
				user: null,
			};
		},
		created() {
		},
		computed: {
			avatar(){
				return this.$storage.get('headportrait')?this.$storage.get('headportrait'):''
			},
		},
		mounted() {
			let sessionTable = this.$storage.get("sessionTable")
			this.$http({
				url: sessionTable + '/session',
				method: "get"
			}).then(({
				data
			}) => {
				if (data && data.code === 0) {
					if(sessionTable == 'yonghu') {
						this.$storage.set('headportrait',data.data.touxiang)
					}
					if(sessionTable == 'cheshang') {
						this.$storage.set('headportrait',data.data.touxiang)
					}
					if(sessionTable == 'users') {
						this.$storage.set('headportrait',data.data.image)
					}
					this.$storage.set('userForm',JSON.stringify(data.data))
					this.user = data.data;
					this.$storage.set('userid',data.data.id);
				} else {
					let message = this.$message
					message.error(data.msg);
				}
			});
		},
		methods: {
			handleCommand(name) {
				if (name == 'logout') {
					this.onLogout()
				} 
				else if (name == 'front') {
					this.onIndexTap()
				}
				else if (name == 'backUp'){
					this.backUp()
				}
				else {
					let router = this.$router
					name = '/'+name
					router.push(name)
				}
			},
			onLogout() {
				let storage = this.$storage
				let router = this.$router
				storage.clear()
				this.$store.dispatch('tagsView/delAllViews')
				router.replace({
					name: "login"
				});
			},
			onIndexTap(){
				window.location.href = `${this.$base.indexUrl}`
			},
			backUp() {
				this.$confirm('是否备份数据库?', '数据备份提示', {
					confirmButtonText: '是',
					cancelButtonText: '否',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: '/mysqldump',
						method: "get"
					}).then(({
						data
					}) => {
						if (data) {
							const binaryData = [];
							binaryData.push(data);
							const objectUrl = window.URL.createObjectURL(new Blob(binaryData, {
								type: 'application/pdf;chartset=UTF-8'
							}))
							const a = document.createElement('a')
							a.href = objectUrl
							a.download = 'mysql.dmp'
							// a.click()
							// 下面这个写法兼容火狐
							a.dispatchEvent(new MouseEvent('click', {
								bubbles: true,
								cancelable: true,
								view: window
							}))
							window.URL.revokeObjectURL(data)
							message.message("数据备份成功")
						} else {
							let message = this.$message
							message.error(data.msg);
						}
					});
				});
			},
		}
	};
</script>


<style lang="css" scoped>
	.navbar {
		.title {
			top: 13px;
			left: 260px;
			display: block;
			position: absolute;
			.title-name {
				padding: 0;
				color: #1a3194;
				font-weight: 600;
				font-size: 20px;
				line-height: 44px;
				float: left;
			}
		}
		.dropdown-box {
			color: inherit;
			display: flex;
			font-size: inherit;
			right: 20px;
			.el-dropdown-link {
				display: flex;
				align-items: center;
				cursor: pointer;
				padding: 4px 8px;
				border-radius: 20px;
				transition: all 0.3s ease;
				
				&:hover {
					background-color: rgba(26, 49, 148, 0.1);
				}
				
				.el-image {
					border-radius: 100%;
					margin: 0 10px;
					display: inline-block;
					width: 42px;
					height: 42px;
					overflow: hidden;
					box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
					border: 2px solid #fff;
					transition: all 0.3s ease;
					
					&:hover {
						transform: scale(1.05);
						box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
					}
				}
				/* 深度选择器修复头像缩放问题 */
				:deep(.el-image__inner) {
					width: 100%;
					height: 100%;
					object-fit: contain;
				}
				.label {
					color: inherit;
					display: none;
					font-size: inherit;
					line-height: 32px;
				}
				.nickname {
					color: inherit;
					font-size: inherit;
					line-height: 32px;
					font-weight: 500;
				}
				.iconfont {
					margin: 0 0 0 5px;
					color: #1a3194;
					font-size: 14px;
					transition: transform 0.3s ease;
				}
				
				&:hover .iconfont {
					transform: translateY(2px);
				}
			}
			.top-el-dropdown-menu {
				border: none;
				border-radius: 8px;
				padding: 8px 0;
				box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
				margin: 18px 0;
				background: #FFF;
				min-width: 120px;
				
				li.el-dropdown-menu__item {
					cursor: pointer;
					padding: 10px 20px;
					margin: 2px 6px;
					outline: 0;
					color: #333;
					background: #fff;
					font-size: 14px;
					line-height: 20px;
					list-style: none;
					border-radius: 6px;
					transition: all 0.25s ease;
					font-weight: 500;
					
					.iconfont {
						margin: 0 12px 0 0;
						color: #1a3194;
						font-size: 16px;
						transition: all 0.25s ease;
					}
					
					&:hover {
						background: rgba(26, 49, 148, 0.05);
						color: #1a3194;
						transform: translateX(2px);
						
					.iconfont {
							color: #1a3194;
							transform: scale(1.1);
						}
				}
					
					&.item1 .iconfont {
						color: #1a3194;
					}
					
					&.item2 .iconfont {
						color: #409EFF;
					}
					
					&.item3 .iconfont {
						color: #409EFF;
				}
					
					&.item4 .iconfont {
						color: #F56C6C; /* 退出按钮的图标颜色 */
					}
				}
				
				/* 在下拉菜单项之间添加分隔符 */
				&::after {
					content: "";
					position: absolute;
					bottom: 46px;
					left: 10px;
					right: 10px;
					height: 1px;
					background: rgba(0, 0, 0, 0.05);
				}
			}
		}
	}
</style>
