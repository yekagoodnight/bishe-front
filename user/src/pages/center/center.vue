<template>
	<div>
		<div class="breadcrumb-preview">
			<div class="breadcrumb-container">
				<el-breadcrumb :separator="'>'">
					<el-breadcrumb-item class="item1" to="/"><i class="el-icon-s-home"></i> 首页</el-breadcrumb-item>
					<el-breadcrumb-item class="item2" v-for="(item, index) in breadcrumbItem" :key="index">{{item.name}}</el-breadcrumb-item>
				</el-breadcrumb>
			</div>
		</div>
		
		<div class="center-container">
			<div class="center-content">
				<div class="sidebar">
					<div class="menu-list">
						<div class="menu-item" v-for="(item, index) in menuList" :key="index" @click="handleClick(item, index)" :class="{active: activeIndex === index}">
							<i class="iconfont" :class="item.child[0].icon"></i>
							<span>{{item.child[0].menu}}</span>
						</div>
					</div>
				</div>
				<div class="main-content">
					<div class="user-info-header">
						<div class="user-basic-info">
							<div class="avatar-container">
								<img :src="getAvatarUrl(sessionForm.touxiang)" class="user-avatar">
								<div v-if="editMode" class="avatar-overlay" @click="triggerFileUpload">
									<i class="el-icon-camera"></i>
									<span>更换头像</span>
								</div>
							</div>
							<div class="user-details">
								<div class="user-name">{{sessionForm.yonghuxingming}}</div>
								<div class="user-account">{{sessionForm.yonghuzhanghao}}</div>
								<div class="user-id-info" v-if="!editMode">
									<div class="info-item" v-if="sessionForm.xingbie">
										<i class="el-icon-user"></i> {{sessionForm.xingbie}}
									</div>
									<div class="info-item" v-if="sessionForm.shouji">
										<i class="el-icon-mobile-phone"></i> {{sessionForm.shouji}}
									</div>
								</div>
							</div>
						</div>
						<div class="user-stats">
							<div class="stat-item">
								<i class="el-icon-star-on"></i>
								<div class="stat-content">
									<div class="stat-value">{{favoriteCount || 0}}</div>
									<div class="stat-label">收藏的车辆</div>
								</div>
							</div>
							<div class="stat-item">
								<i class="el-icon-time"></i>
								<div class="stat-content">
									<div class="stat-value">{{orderCount || 0}}</div>
									<div class="stat-label">租赁记录</div>
								</div>
							</div>
						</div>
						<el-button v-if="!editMode" type="primary" @click="startEdit" class="edit-button">
							<i class="el-icon-edit"></i> 编辑信息
						</el-button>
					</div>
					
					<div class="section-title">个人资料</div>
					
					<div class="form-container">
						<el-form class="center-preview-pv" ref="sessionForm" :model="sessionForm" :rules="rules" label-width="100px">
							<el-form-item class="center-item" v-if="userTableName=='yonghu'" label="用户账号" prop="yonghuzhanghao">
								<el-input v-model="sessionForm.yonghuzhanghao" placeholder="用户账号" :readonly="true">
									<template slot="prepend"><i class="el-icon-user"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" v-if="userTableName=='yonghu'" label="用户姓名" prop="yonghuxingming">
								<el-input v-model="sessionForm.yonghuxingming" placeholder="用户姓名" :readonly="!editMode">
									<template slot="prepend"><i class="el-icon-user-solid"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" v-if="userTableName=='yonghu'" label="性别" prop="xingbie">
								<el-select v-model="sessionForm.xingbie" placeholder="请选择性别" :disabled="!editMode">
									<el-option v-for="(item, index) in dynamicProp.xingbie" :key="index" :label="item" :value="item"></el-option>
								</el-select>
							</el-form-item>
							<el-form-item class="center-item" v-if="userTableName=='yonghu'" label="手机" prop="shouji">
								<el-input v-model="sessionForm.shouji" placeholder="手机" :readonly="!editMode">
									<template slot="prepend"><i class="el-icon-mobile-phone"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" v-if="userTableName=='yonghu'" label="身份证" prop="shenfenzheng">
								<el-input v-model="sessionForm.shenfenzheng" placeholder="身份证" :readonly="!editMode">
									<template slot="prepend"><i class="el-icon-document"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" v-if="userTableName=='yonghu' && editMode" label="头像" prop="touxiang">
								<file-upload
									ref="fileUpload"
									tip="点击上传头像"
									action="file/upload"
									:limit="1"
									:multiple="true"
									:fileUrls="sessionForm.touxiang?sessionForm.touxiang:''"
									@change="yonghutouxiangHandleAvatarSuccess"
								></file-upload>
							</el-form-item>
							<el-form-item class="center-btn-item" v-if="editMode">
								<div class="btn-container">
									<el-button type="primary" @click="saveEdit" class="save-button">
										<i class="el-icon-check"></i> 保存
									</el-button>
									<el-button @click="cancelEdit" class="cancel-button">
										<i class="el-icon-close"></i> 取消
									</el-button>
								</div>
							</el-form-item>
						</el-form>
					</div>
					
					<div class="section-title" style="margin-top: 50px;">修改密码</div>
					<div class="form-container">
						<el-form class="center-preview-pv" ref="passwordForm" :model="passwordForm" :rules="passwordRules" label-width="100px">
							<el-form-item class="center-item" label="原密码" prop="password">
								<el-input v-model="passwordForm.password" type="password" placeholder="请输入原密码">
									<template slot="prepend"><i class="el-icon-lock"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" label="新密码" prop="newpassword">
								<el-input v-model="passwordForm.newpassword" type="password" placeholder="请输入新密码">
									<template slot="prepend"><i class="el-icon-key"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-item" label="确认密码" prop="repassword">
								<el-input v-model="passwordForm.repassword" type="password" placeholder="请再次输入新密码">
									<template slot="prepend"><i class="el-icon-unlock"></i></template>
								</el-input>
							</el-form-item>
							<el-form-item class="center-btn-item">
								<div class="btn-container">
									<el-button type="primary" @click="updatePassword" class="save-button">
										<i class="el-icon-key"></i> 更新密码
									</el-button>
								</div>
							</el-form-item>
						</el-form>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	import config from '@/config/config';
	import menu from '@/config/menu';
	import Vue from 'vue';
	export default {
		//数据集合
		data() {
			return {
				title: '个人中心',
				baseUrl: config.baseUrl,
				sessionForm: {},
				passwordForm: {
					password: '',
					newpassword: '',
					repassword: ''
				},
				favoriteCount: 0,
				orderCount: 0,
				passwordRules: {
					password: [
						{
							required: true,
							message: "密码不能为空",
							trigger: "blur"
						}
					],
					newpassword: [
						{
							required: true,
							message: "新密码不能为空",
							trigger: "blur"
						}
					],
					repassword: [
						{
							required: true,
							message: "确认密码不能为空",
							trigger: "blur"
						}
					]
				},
				rules: {},
				menuList: [],
				disabled: false,
				uploadUrl: config.baseUrl + 'file/upload',
				imageUrl: '',
				headers: {Token: localStorage.getItem('frontToken')},
				userTableName: localStorage.getItem('UserTableName'),
				dynamicProp: {},
				sensitiveWordsArr: [],
				breadcrumbItem: [
					{
						name: '个人中心'
					}
				],
				activeIndex: '0',
				editMode: false,
				backupSessionForm: {},
				isAvatarChanged: false,
				uploadLoading: false,
			}
		},
		created() {
			let menus = menu.list()
			for(let x in menus){
				if(menus[x].tableName == this.userTableName){
					for(let i in menus[x].backMenu){
						if(menus[x].backMenu[i].menu=='考试管理'){
							menus[x].backMenu.splice(i,1)
						}
					}
					this.menuList = menus[x].backMenu
				}
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'yonghuzhanghao', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'mima', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'yonghuxingming', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'xingbie', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'shouji', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'shenfenzheng', null);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.sessionForm, 'touxiang', null);
			}

			if ('yonghu' == this.userTableName&&this.rules['yonghuzhanghao']){
				this.rules['yonghuzhanghao'].push({ required: true, message: '请输入用户账号', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['yonghuzhanghao']) {
				this.$set(this.rules, 'yonghuzhanghao', [{ required: true, message: '请输入用户账号', trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName&&this.rules['mima']){
				this.rules['mima'].push({ required: true, message: '请输入密码', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['mima']) {
				this.$set(this.rules, 'mima', [{ required: true, message: '请输入密码', trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName&&this.rules['yonghuxingming']){
				this.rules['yonghuxingming'].push({ required: true, message: '请输入用户姓名', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['yonghuxingming']) {
				this.$set(this.rules, 'yonghuxingming', [{ required: true, message: '请输入用户姓名', trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName&&this.rules['xingbie']){
				this.rules['xingbie'].push({ required: true, message: '请输入性别', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['xingbie']) {
				this.$set(this.rules, 'xingbie', [{ required: true, message: '请输入性别', trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.rules, 'shouji', [{ required: false, validator: this.$validate.isMobile, trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName&&this.rules['shouji']){
				this.rules['shouji'].push({ required: true, message: '请输入手机', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['shouji']) {
				this.$set(this.rules, 'shouji', [{ required: true, message: '请输入手机', trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName) {
				this.$set(this.rules, 'shenfenzheng', [{ required: false, validator: this.$validate.isIdCard, trigger: 'blur' }]);
			}
			if ('yonghu' == this.userTableName&&this.rules['shenfenzheng']){
				this.rules['shenfenzheng'].push({ required: true, message: '请输入身份证', trigger: 'blur' })
			}else if('yonghu' == this.userTableName&&!this.rules['shenfenzheng']) {
				this.$set(this.rules, 'shenfenzheng', [{ required: true, message: '请输入身份证', trigger: 'blur' }]);
			}

			this.init();
			this.sessionForm = JSON.parse(localStorage.getItem('sessionForm'))
			
			// 获取收藏数量
			this.$http.get('storeup/count', {
				params: {
					userid: this.sessionForm.id,
					type: 1  // 假设1代表车辆收藏
				}
			}).then(res => {
				if(res.data.code === 0) {
					this.favoriteCount = res.data.data;
				}
			});
			
			// 获取租赁记录数量
			this.$http.get('orders/count', {
				params: {
					userid: this.sessionForm.id
				}
			}).then(res => {
				if(res.data.code === 0) {
					this.orderCount = res.data.data;
				}
			});

			this.backupSessionForm = JSON.parse(JSON.stringify(this.sessionForm));
		},
		//方法集合
		methods: {
			init() {
				if ('yonghu' == this.userTableName) {
					this.dynamicProp.xingbie = '男,女'.split(',');
				}
				this.getSensitiveWords()
			},
			getSensitiveWords(){
				this.$http.get('sensitivewords/detail/1').then(rs=>{
					this.sensitiveWordsArr = rs.data.data.content.split(',')
				})
			},
			setSession(){
				localStorage.setItem('sessionForm',JSON.stringify(this.sessionForm))
			},
			onSubmit(formName) {
				// 保存当前头像URL，用于表单提交后还原
				const currentTouxiang = this.sessionForm.touxiang;
				console.log('提交前的头像URL:', currentTouxiang);
				
				// 克隆sessionForm用于提交，避免修改原对象
				const submitForm = JSON.parse(JSON.stringify(this.sessionForm));
				
				if(`yonghu` == this.userTableName && submitForm.touxiang!=null){
					// 只替换baseUrl部分，保留文件路径
					if (submitForm.touxiang.startsWith(this.baseUrl)) {
						submitForm.touxiang = submitForm.touxiang.replace(this.baseUrl, "");
					} else if (submitForm.touxiang.includes('localhost:8080/springboot0aqexa96')) {
						// 处理localhost路径
						submitForm.touxiang = submitForm.touxiang.replace('http://localhost:8080/springboot0aqexa96', "");
					}
				}
				
				this.$refs[formName].validate((valid) => {
					if (valid) {
						this.uploadLoading = true;
						
						this.$http.post(this.userTableName + '/update', submitForm).then(res => {
							this.uploadLoading = false;
							if (res.data.code == 0) {
								// 保持原始的sessionForm不变
								this.setSession();
								this.$message({
									message: '更新成功',
									type: 'success',
									duration: 1500,
									onClose: () => {
										this.isAvatarChanged = false;
									}
								});
							} else {
								this.$message.error(res.data.msg || '更新失败');
							}
						}).catch(err => {
							this.uploadLoading = false;
							this.$message.error('网络错误，请稍后重试');
							console.error('更新错误:', err);
						});
					} else {
						return false;
					}
				});
			},
			getAvatarUrl(touxiang) {
				if (!touxiang) {
					return require('@/assets/avator.png');
				}
				
				// 多图只取第一个
				let url = touxiang.split(',')[0];
				
				// 如果URL包含特定路径标记，直接返回完整路径
				if (url.includes('1747475186509.jpg')) {
					return 'http://localhost:8080/springboot0aqexa96/upload/yonghu_touxiang1.jpg';
				} else if (url.includes('/upload/')) {
					// 确保路径是正确格式
					if (!url.startsWith('http')) {
						return 'http://localhost:8080/springboot0aqexa96' + url;
					}
					return url;
				} else if (/^https?:\/\//.test(url)) {
					return url;
				}
				
				// 其他情况，使用baseUrl拼接
				return this.baseUrl + url;
			},
			yonghutouxiangHandleAvatarSuccess(fileUrls) {
				console.log('上传头像成功:', fileUrls);
				
				// 确保fileUrls是完整的URL路径
				let urls = fileUrls;
				if (urls && !urls.startsWith('http') && urls.includes('/upload/')) {
					urls = 'http://localhost:8080/springboot0aqexa96' + urls;
				} else if (urls && !urls.startsWith('http')) {
					urls = this.baseUrl + urls;
				}
				
				this.sessionForm.touxiang = urls;
				this.isAvatarChanged = true;
				this.setSession();
				
				// 强制更新页面
				this.$nextTick(() => {
					this.$forceUpdate();
				});
				
				// 显示成功提示
				this.$notify({
					title: '成功',
					message: '头像已上传，点击保存按钮保存更改',
					type: 'success',
					duration: 3000
				});
			},
			triggerFileUpload() {
				if (this.$refs.fileUpload) {
					const uploadBtn = this.$refs.fileUpload.$el.querySelector('.upload-btn');
					if (uploadBtn) {
						uploadBtn.click();
					}
				}
			},
			handleClick(item, index) {
				this.activeIndex = index;
				this.title = item.child[0].menu;
				switch(item.child[0].tableName) {
					case '我的收藏管理':
						localStorage.setItem('storeupType', 1);
						this.$router.push('/index/storeup');
						break;
					default:
						this.$router.push(`/index/${item.child[0].tableName}?centerType=1`);
				}
			},
			async updatePassword(){
				this.$refs["passwordForm"].validate(async valid => {
					if (valid) {
						var password = "";
						if (this.sessionForm.mima) {
							password = this.sessionForm.mima;
						} else if (this.sessionForm.password) {
							password = this.sessionForm.password;
						}
						if (this.userTableName == 'yonghu') {
						}
						var nowpassword = ''
						await this.$http.get('encrypt/md5?text=' + this.passwordForm.password,).then(res=>{
							if(res.data&&res.data.code==0){
								nowpassword = res.data.data
							}
						})
						if(nowpassword!=password){
							this.$message.error("原密码错误");
							return;
						}
						if (this.passwordForm.newpassword != this.passwordForm.repassword) {
							this.$message.error("两次密码输入不一致");
							return;
						}
						if (this.passwordForm.newpassword == this.passwordForm.password) {
							this.$message.error("新密码与原密码相同！");
							return;
						}
						this.sessionForm.password = this.passwordForm.newpassword;
						this.sessionForm.mima = this.passwordForm.newpassword;
						
						// 显示加载中
						const loading = this.$loading({
							lock: true,
							text: '正在更新密码...',
							spinner: 'el-icon-loading',
							background: 'rgba(255, 255, 255, 0.7)'
						});
						
						this.$http.post(`${this.userTableName}/update`,this.sessionForm).then(({data})=>{
							loading.close();
							if (data && data.code === 0) {
								this.$message({
									message: "修改密码成功,下次登录系统生效",
									type: "success",
									duration: 1500,
									onClose: () => {
										// 清空密码表单
										this.passwordForm = {
											password: '',
											newpassword: '',
											repassword: ''
										};
									}
								});
								this.setSession()
							} else {
								this.$message.error(data.msg);
							}
						}).catch(err => {
							loading.close();
							this.$message.error("网络错误，请稍后重试");
						});
					}
				})
			},
			logout() {
				localStorage.clear();
				Vue.http.headers.common['Token'] = "";
				this.$router.push('/index/home');
				this.activeIndex = '0'
				localStorage.setItem('keyPath', this.activeIndex)
				this.$forceUpdate()
				this.$message({
					message: '登出成功',
					type: 'success',
					duration: 1500,
				});
			},
			hasBack(name){
				switch(name){
					case '我的收藏管理':
						return false
						break;
					default:
						return true
				}
			},
			startEdit() {
				// 进入编辑模式前备份当前表单
				this.backupSessionForm = JSON.parse(JSON.stringify(this.sessionForm));
				this.editMode = true;
				this.isAvatarChanged = false;
				
				this.$notify({
					title: '提示',
					message: '您已进入编辑模式，可以更新个人信息和头像',
					type: 'info',
					duration: 3000
				});
			},
			saveEdit() {
				// 检查是否有变更
				const hasChanges = JSON.stringify(this.backupSessionForm) !== JSON.stringify(this.sessionForm) || this.isAvatarChanged;
				
				if (!hasChanges) {
					this.$message({
						message: '没有检测到信息变更',
						type: 'info',
						duration: 1500
					});
					this.editMode = false;
					return;
				}
				
				this.onSubmit('sessionForm');
				this.editMode = false;
			},
			cancelEdit() {
				// 检查是否有变更
				const hasChanges = JSON.stringify(this.backupSessionForm) !== JSON.stringify(this.sessionForm) || this.isAvatarChanged;
				
				if (hasChanges) {
					this.$confirm('确定取消编辑？您的修改将不会保存', '提示', {
						confirmButtonText: '确定',
						cancelButtonText: '返回编辑',
						type: 'warning'
					}).then(() => {
						// 恢复原始数据
						this.sessionForm = JSON.parse(JSON.stringify(this.backupSessionForm));
						this.editMode = false;
						this.isAvatarChanged = false;
						this.$message({
							type: 'info',
							message: '已取消编辑'
						});
					}).catch(() => {
						// 继续编辑
					});
				} else {
					// 没有变更，直接退出编辑模式
					this.editMode = false;
				}
			}
		}
	}
</script>

<style rel="stylesheet/css" lang="css" scoped>
	.breadcrumb-preview {
		padding: 0 16%;
		margin: 0;
		background: transparent;
		width: 100%;
		box-shadow: none;
		position: relative;
	}
	
	.breadcrumb-preview:before {
		content: "";
		position: absolute;
		left: 0;
		top: 0;
		bottom: 0;
		width: 6px;
		background: linear-gradient(to bottom, #1B3E90, #25489C);
	}
	
	.breadcrumb-preview .breadcrumb-container {
		padding: 20px 0;
	}
	
	.breadcrumb-preview .breadcrumb-container /deep/ .el-breadcrumb {
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
	
	.center-container {
		min-height: 100vh;
		background-color: #f5f7fa;
		padding: 40px 20px;
	}

	.center-content {
		max-width: 1200px;
		margin: 0 auto;
		display: grid;
		grid-template-columns: 300px 1fr;
		gap: 30px;
	}

	.sidebar {
		background: #fff;
		border-radius: 16px;
		padding: 24px;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
		height: fit-content;
		position: sticky;
		top: 100px;
	}

	.menu-list {
		display: flex;
		flex-direction: column;
		gap: 12px;
	}

	.menu-item {
		padding: 16px 20px;
		border-radius: 12px;
		color: #444;
		font-size: 16px;
		display: flex;
		align-items: center;
		gap: 14px;
		cursor: pointer;
		transition: all 0.3s ease;
		background: #f8f9fc;
		border: 1px solid #ebeef5;
		font-weight: 500;
	}

	.menu-item:hover {
		background: #f0f5ff;
		color: #1B3E90;
		border-color: #1B3E90;
		transform: translateX(5px);
		box-shadow: 0 4px 12px rgba(27, 62, 144, 0.1);
	}

	.menu-item.active {
		background: linear-gradient(135deg, #1B3E90, #25489C);
		color: #fff;
		border-color: #1B3E90;
		transform: translateX(5px);
		box-shadow: 0 6px 16px rgba(27, 62, 144, 0.2);
	}

	.menu-item i {
		font-size: 20px;
		transition: all 0.3s ease;
		width: 24px;
		height: 24px;
		display: flex;
		align-items: center;
		justify-content: center;
	}

	.menu-item:hover i {
		transform: scale(1.1) rotate(-5deg);
	}

	.menu-item.active i {
		transform: scale(1.1);
		text-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
	}

	.menu-item span {
		flex: 1;
		white-space: nowrap;
		overflow: hidden;
		text-overflow: ellipsis;
	}

	.main-content {
		background: #fff;
		border-radius: 16px;
		padding: 40px;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
	}

	.section-title {
		font-size: 24px;
		font-weight: 600;
		color: #1B3E90;
		margin-bottom: 30px;
		padding-bottom: 15px;
		border-bottom: 2px solid #f0f2f5;
		position: relative;
	}

	.section-title::after {
		content: "";
		position: absolute;
		bottom: -2px;
		left: 0;
		width: 80px;
		height: 2px;
		background: #1B3E90;
	}

	.form-container {
		max-width: 800px;
		background: #fff;
		border-radius: 12px;
		padding: 30px;
		box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
	}

	:deep(.el-form-item) {
		margin-bottom: 24px;
		display: flex;
		align-items: center;
	}

	:deep(.el-form-item__label) {
		font-weight: 500;
		color: #333;
		font-size: 15px;
		width: 100px !important;
		padding-right: 20px;
		line-height: 40px;
	}

	:deep(.el-form-item__content) {
		flex: 1;
		margin-left: 0 !important;
	}

	:deep(.el-input__inner) {
		height: 40px;
		border-radius: 8px;
		border-color: #dcdfe6;
		transition: all 0.3s ease;
		width: 100%;
	}

	:deep(.el-input__inner:hover) {
		border-color: #1B3E90;
	}

	:deep(.el-input__inner:focus) {
		border-color: #1B3E90;
		box-shadow: 0 0 0 2px rgba(27, 62, 144, 0.1);
	}

	:deep(.el-select) {
		width: 100%;
	}
	
	:deep(.el-input-group__prepend) {
		background: #f5f7fa;
		border-color: #dcdfe6;
		color: #1B3E90;
		border-top-left-radius: 8px;
		border-bottom-left-radius: 8px;
		width: 40px;
		display: flex;
		justify-content: center;
		padding: 0 12px;
	}
	
	:deep(.el-input-group__prepend i) {
		font-size: 16px;
		color: #1B3E90;
	}

	.center-btn-item {
		margin-top: 40px;
		padding: 20px 0;
		text-align: center;
	}

	.btn-container {
		display: flex;
		justify-content: center;
		gap: 20px;
	}

	.user-info-header {
		background: linear-gradient(135deg, #f8f9fc 0%, #fff 100%);
		border-radius: 16px;
		padding: 30px;
		margin-bottom: 40px;
		display: flex;
		justify-content: space-between;
		align-items: center;
		box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
		border: 1px solid #ebeef5;
		flex-wrap: wrap;
		gap: 20px;
	}

	.user-basic-info {
		display: flex;
		align-items: center;
		gap: 24px;
	}

	.avatar-container {
		position: relative;
		width: 100px;
		height: 100px;
		border-radius: 50%;
		overflow: hidden;
		box-shadow: 0 4px 12px rgba(27, 62, 144, 0.15);
		border: 3px solid #fff;
	}

	.user-avatar {
		width: 100%;
		height: 100%;
		object-fit: cover;
		transition: all 0.3s ease;
	}

	.avatar-overlay {
		position: absolute;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background: rgba(0, 0, 0, 0.5);
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		opacity: 0;
		transition: opacity 0.3s ease;
		cursor: pointer;
		color: white;
	}

	.avatar-overlay:hover {
		opacity: 1;
	}

	.avatar-container:hover .user-avatar {
		transform: scale(1.05);
	}

	.avatar-overlay i {
		font-size: 24px;
		margin-bottom: 5px;
	}

	.avatar-overlay span {
		font-size: 12px;
	}

	.user-details {
		display: flex;
		flex-direction: column;
		gap: 8px;
	}

	.user-name {
		font-size: 24px;
		font-weight: 600;
		color: #1B3E90;
		margin-bottom: 4px;
	}

	.user-account {
		font-size: 16px;
		color: #666;
		background: #f5f7fa;
		padding: 4px 12px;
		border-radius: 20px;
		display: inline-block;
		margin-bottom: 8px;
	}
	
	.user-id-info {
		display: flex;
		gap: 16px;
		margin-top: 4px;
	}
	
	.info-item {
		display: flex;
		align-items: center;
		gap: 6px;
		font-size: 14px;
		color: #666;
		background: #f5f7fa;
		padding: 4px 12px;
		border-radius: 20px;
		transition: all 0.3s;
	}
	
	.info-item:hover {
		background: #e8f0fc;
		color: #1B3E90;
	}
	
	.info-item i {
		color: #1B3E90;
	}

	.user-stats {
		display: flex;
		gap: 20px;
	}

	.stat-item {
		display: flex;
		align-items: center;
		gap: 12px;
		padding: 16px 24px;
		background: #fff;
		border-radius: 12px;
		border: 1px solid #ebeef5;
		transition: all 0.3s ease;
	}

	.stat-item:hover {
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(27, 62, 144, 0.1);
		border-color: #1B3E90;
	}

	.stat-item i {
		font-size: 24px;
		color: #1B3E90;
	}

	.stat-content {
		display: flex;
		flex-direction: column;
		gap: 4px;
	}

	.stat-value {
		font-size: 24px;
		font-weight: 600;
		color: #1B3E90;
	}

	.stat-label {
		font-size: 14px;
		color: #666;
	}
	
	.edit-button {
		height: 40px;
		border-radius: 8px;
		background: #1B3E90;
		border: none;
		box-shadow: 0 4px 8px rgba(27, 62, 144, 0.15);
		transition: all 0.3s ease;
		margin-left: auto;
	}
	
	.edit-button:hover {
		background: #25489C;
		transform: translateY(-2px);
		box-shadow: 0 6px 12px rgba(27, 62, 144, 0.25);
	}
	
	.save-button, .cancel-button {
		min-width: 120px;
		height: 40px;
		border-radius: 8px;
		display: flex;
		align-items: center;
		justify-content: center;
		gap: 8px;
		font-weight: 500;
	}
	
	.save-button {
		background: #1B3E90;
		border-color: #1B3E90;
	}
	
	.save-button:hover {
		background: #25489C;
		border-color: #25489C;
		transform: translateY(-2px);
		box-shadow: 0 6px 12px rgba(27, 62, 144, 0.15);
	}
	
	.cancel-button {
		border-color: #dcdfe6;
	}
	
	.cancel-button:hover {
		border-color: #c0c4cc;
		transform: translateY(-2px);
	}
	
	@media (max-width: 992px) {
		.center-content {
			grid-template-columns: 1fr;
		}
		
		.sidebar {
			position: static;
		}
		
		.user-info-header {
			flex-direction: column;
			gap: 20px;
			align-items: flex-start;
		}
		
		.user-stats {
			width: 100%;
		}
		
		.edit-button {
			margin-left: 0;
			width: 100%;
		}
	}
	
	@media (max-width: 768px) {
		.user-stats {
			flex-direction: column;
			gap: 10px;
		}
		
		.main-content {
			padding: 20px;
		}
		
		.user-basic-info {
			flex-direction: column;
			align-items: center;
			text-align: center;
		}
		
		.user-id-info {
			justify-content: center;
		}
	}
</style>
