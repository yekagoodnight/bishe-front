<template>
	<div class="messages-page">
		<div class="breadcrumb-preview">
			<div class="breadcrumb-container">
				<el-breadcrumb :separator="'>'">
					<el-breadcrumb-item class="item1" to="/"><i class="el-icon-s-home"></i> 首页</el-breadcrumb-item>
					<el-breadcrumb-item class="item2" v-for="(item, index) in breadcrumbItem" :key="index">{{item.name}}</el-breadcrumb-item>
				</el-breadcrumb>
			</div>
		</div>
		
		<div class="main-container">
			<div class="section-title">
				<div class="title-box">
					<span class="title">留言板</span>
					<span class="subhead">MESSAGES</span>
				</div>
				<div class="title-line"></div>
			</div>

			<div class="message-form-container">
				<el-form :model="form" :rules="rules" ref="form" label-width="65px" label-position="left">
					<div class="form-content">
						<div class="editor-section">
							<el-form-item label="留言" prop="content">
								<editor
									v-model="form.content" 
									class="message-editor" 
									action="file/upload">
								</editor>
							</el-form-item>
						</div>
						<div class="upload-section">
							<el-form-item label="图片" prop="cpicture">
								<file-upload
									class="image-uploader"
									tip="支持 jpg、png、gif 格式，单张图片不超过 5MB"
									action="file/upload"
									:limit="1"
									:multiple="true"
									:fileUrls="form.cpicture?form.cpicture:''"
									@change="cpictureUploadChange"
								>
									<template #default>
										<div class="upload-content">
											<i class="el-icon-upload"></i>
											<div class="upload-text">点击或拖拽图片到此处上传</div>
											<div class="upload-tip">支持 jpg、png、gif 格式</div>
										</div>
									</template>
								</file-upload>
							</el-form-item>
						</div>
					</div>
					<div class="form-buttons-container">
						<el-form-item class="form-buttons">
							<el-button type="primary" @click="submitForm('form')">发布留言</el-button>
							<el-button @click="resetForm('form')">重置</el-button>
						</el-form-item>
					</div>
				</el-form>
			</div>

			<div class="messages-list">
				<div v-for="item in infoList" :key="item.id" class="message-item">
					<div class="message-header">
						<div class="user-info">
							<el-avatar v-if="item.avatarurl" :size="50" :src="$config.baseUrl + item.avatarurl"></el-avatar>
							<el-avatar v-else :size="50" :src="require('@/assets/touxiang.png')"></el-avatar>
							<span class="username">{{item.username}}</span>
						</div>
					</div>
					<div class="message-content">
						<div class="content-text ql-snow ql-editor" v-html="item.content"></div>
						<div v-if="item.cpicture" class="content-image">
							<el-image 
								:src="$config.baseUrl+item.cpicture"
								:preview-src-list="[$config.baseUrl+item.cpicture]">
							</el-image>
						</div>
					</div>
					<div v-if="item.reply" class="reply-content">
						<div class="reply-header">管理员回复：</div>
						<div class="reply-text ql-snow ql-editor" v-html="item.reply"></div>
						<div v-if="item.rpicture" class="reply-image">
							<el-image 
								:src="$config.baseUrl+item.rpicture"
								:preview-src-list="[$config.baseUrl+item.rpicture]">
							</el-image>
						</div>
					</div>
				</div>
			</div>

			<el-pagination
				background
				class="pagination"
				:pager-count="7"
				:page-size="pageSize"
				:page-sizes="pageSizes"
				prev-text="<"
				next-text=">"
				:hide-on-single-page="true"
				:layout='["prev","pager","next","jumper"].join()'
				:total="total"
				@current-change="curChange"
				@prev-click="prevClick"
				@next-click="nextClick"
			></el-pagination>
		</div>
	</div>
</template>

<script>
	export default {
		//数据集合
		data() {
			return {
				layouts: '',
				breadcrumbItem: [
					{
						name: '留言板'
					}
				],
				form: {
					content: '',
					userid: localStorage.getItem('frontUserid'),
					username: localStorage.getItem('username')
				},
				total: 1,
				pageSize: 20,
				pageSizes: [],
				totalPage: 1,
				rules: {
					content: [
						{ required: true, message: '请输入内容', trigger: 'blur' }
					]
				},
				infoList: [],
				sensitiveWordsArr: [],
			}
		},
		created() {
			this.getInfo(1);
			this.getSensitiveWords()
		},
		//方法集合
		methods: {
			getSensitiveWords(){
				this.$http.get('sensitivewords/detail/1').then(rs=>{
					this.sensitiveWordsArr = rs.data.data.content.split(',')
				})
			},
			getInfo(page) {
				this.$http.get('messages/list', {params: {page, limit: this.pageSize,sort:"addtime", order:"desc"}}).then(res => {
					if (res.data.code == 0) {
						this.infoList = res.data.data.list;
						this.total = res.data.data.total;
						this.pageSize = Number(res.data.data.pageSize);
						this.totalPage = res.data.data.totalPage;
						if(this.pageSizes.length==0){
							this.pageSizes = [this.pageSize, this.pageSize*2, this.pageSize*3, this.pageSize*5];
						}
					}
				});
			},
			curChange(page) {
				this.getInfo(page);
			},
			prevClick(page) {
				this.getInfo(page);
			},
			nextClick(page) {
				this.getInfo(page);
			},
			cpictureUploadChange(fileUrls) {
				this.form.cpicture = fileUrls;
			},
			submitForm(formName) {
				this.$refs[formName].validate((valid) => {
					if (valid) {
						if(this.form.cpicture!=null) {
							this.form.cpicture = this.form.cpicture.replace(new RegExp(this.$config.baseUrl,"g"),"");
						}
						this.form.avatarurl = localStorage.getItem('frontHeadportrait')?localStorage.getItem('frontHeadportrait'):'';
						for(var i=0; i<this.sensitiveWordsArr.length; i++){
							//全局替换
							var reg = new RegExp(this.sensitiveWordsArr[i],"g");
							//判断内容中是否包括敏感词
							if (this.form.content.indexOf(this.sensitiveWordsArr[i]) > -1) {
								// 将敏感词替换为 **
								this.form.content = this.form.content.replace(reg,"**");
							}
						}
						this.$http.post('messages/add', this.form).then(res => {
							if (res.data.code == 0) {
								this.$message({
									type: 'success',
									message: '留言成功!',
									duration: 1500,
									onClose: () => {
										window.location.reload();
									}
								});
							}
						});
					} else {
						return false;
					}
				});
			},
			resetForm(formName) {
				this.$refs[formName].resetFields();
			}
		}
	}
</script>

<style rel="stylesheet/css" lang="css" scoped>
.messages-page {
	min-height: 100vh;
	background-color: #f5f7fa;
}

.breadcrumb-preview {
	padding: 0 16%;
	background: #fff;
	width: 100%;
	box-shadow: 0 2px 12px rgba(0,0,0,0.05);
	position: relative;
}

.breadcrumb-preview::before {
	content: "";
	position: absolute;
	left: 0;
	top: 0;
	bottom: 0;
	width: 4px;
	background: linear-gradient(to bottom, #1B3E90, #25489C);
}

.breadcrumb-container {
	padding: 20px 0;
}

.breadcrumb-container :deep(.el-breadcrumb) {
	font-size: 16px;
}

.breadcrumb-container :deep(.el-breadcrumb__item .el-breadcrumb__inner) {
	color: #606266;
	font-weight: 500;
	transition: all 0.3s;
}

.breadcrumb-container :deep(.el-breadcrumb__item .el-breadcrumb__inner:hover) {
	color: #1B3E90;
	transform: scale(1.05);
}

.breadcrumb-container :deep(.el-breadcrumb__item .el-breadcrumb__inner a) {
	font-weight: 500;
	color: #606266;
	transition: all 0.3s;
}

.breadcrumb-container :deep(.el-breadcrumb__item .el-breadcrumb__inner a:hover) {
	color: #1B3E90;
}

.breadcrumb-container :deep(.el-breadcrumb__item:last-child .el-breadcrumb__inner) {
	color: #1B3E90;
	font-weight: 600;
}

.breadcrumb-container :deep(.el-breadcrumb__separator) {
	color: #909399;
	margin: 0 10px;
}

.main-container {
	width: 100%;
	max-width: 1200px;
	padding: 40px 20px;
	margin: 0 auto;
}

.section-title {
	margin-bottom: 40px;
	text-align: center;
}

.title-box {
	width: 400px;
	height: 80px;
	margin: 0 auto;
	position: relative;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	background: linear-gradient(135deg, #ffffff 0%, #f5f7fa 100%);
	border-radius: 12px;
	box-shadow: 0 4px 20px rgba(27, 62, 144, 0.1);
}

.title {
	color: #1B3E90;
	font-size: 32px;
	font-weight: 600;
	margin-bottom: 4px;
}

.subhead {
	color: #666;
	font-size: 16px;
	text-transform: uppercase;
	letter-spacing: 2px;
}

.title-line {
	position: absolute;
	height: 3px;
	width: 60px;
	background: linear-gradient(90deg, #1B3E90 0%, rgba(27, 62, 144, 0.3) 100%);
	bottom: -3px;
	left: 50%;
	transform: translateX(-50%);
	border-radius: 3px;
}

.message-form-container {
	background: #fff;
	border-radius: 12px;
	padding: 30px;
	box-shadow: 0 4px 20px rgba(0, 0, 0, 0.05);
	margin-bottom: 40px;
}

.form-content {
	display: grid;
	grid-template-columns: 2fr 1fr;
	gap: 30px;
	margin-bottom: 30px;
}

.editor-section {
	background: #fff;
}

.upload-section {
	background: #fff;
	border-radius: 8px;
}

:deep(.el-form-item__label) {
	color: #333;
	font-weight: 500;
}

:deep(.el-input__inner) {
	border-color: #dcdfe6;
	border-radius: 8px;
}

:deep(.el-input__inner:focus) {
	border-color: #1B3E90;
}

:deep(.ql-container) {
	border-radius: 8px;
	min-height: 200px;
}

:deep(.el-upload-dragger) {
	width: 100%;
	height: 200px;
	border: 2px dashed #dcdfe6;
	border-radius: 8px;
	background: #f5f7fa;
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	transition: all 0.3s;
}

:deep(.el-upload-dragger:hover) {
	border-color: #1B3E90;
	background: #f0f5ff;
}

.upload-content {
	text-align: center;
}

.upload-content i {
	font-size: 48px;
	color: #1B3E90;
	margin-bottom: 16px;
}

.upload-text {
	color: #333;
	font-size: 16px;
	margin-bottom: 8px;
}

.upload-tip {
	color: #666;
	font-size: 14px;
}

.form-buttons-container {
	display: flex;
	justify-content: flex-end;
	padding-top: 20px;
	border-top: 1px solid #ebeef5;
}

.form-buttons :deep(.el-button) {
	padding: 12px 30px;
	font-size: 16px;
	border-radius: 8px;
	transition: all 0.3s;
}

.form-buttons :deep(.el-button--primary) {
	background: #1B3E90;
	border-color: #1B3E90;
}

.form-buttons :deep(.el-button--primary:hover) {
	background: #25489C;
	transform: translateY(-2px);
	box-shadow: 0 4px 12px rgba(27, 62, 144, 0.2);
}

.form-buttons :deep(.el-button--default) {
	border-color: #dcdfe6;
	color: #606266;
}

.form-buttons :deep(.el-button--default:hover) {
	border-color: #1B3E90;
	color: #1B3E90;
	transform: translateY(-2px);
	box-shadow: 0 4px 12px rgba(27, 62, 144, 0.1);
}

.messages-list {
	margin-top: 40px;
}

.message-item {
	background: #fff;
	border-radius: 12px;
	padding: 24px;
	margin-bottom: 20px;
	box-shadow: 0 2px 12px rgba(0, 0, 0, 0.05);
	transition: all 0.3s;
}

.message-item:hover {
	transform: translateY(-2px);
	box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
}

.message-header .user-info {
	display: flex;
	align-items: center;
	gap: 12px;
}

.message-header .username {
	font-size: 16px;
	font-weight: 500;
	color: #303133;
}

.message-content {
	margin: 16px 0;
	padding-left: 62px;
}

.message-content .content-text {
	color: #606266;
	line-height: 1.6;
}

.message-content .content-image {
	margin-top: 12px;
}

.message-content .content-image :deep(.el-image) {
	max-width: 300px;
	max-height: 300px;
	border-radius: 4px;
	overflow: hidden;
}

.reply-content {
	margin-top: 20px;
	padding: 16px;
	background: #f5f7fa;
	border-radius: 4px;
}

.reply-content .reply-header {
	color: #1B3E90;
	font-weight: 500;
	margin-bottom: 8px;
}

.reply-content .reply-text {
	color: #606266;
	line-height: 1.6;
}

.reply-content .reply-image {
	margin-top: 12px;
}

.reply-content .reply-image :deep(.el-image) {
	max-width: 300px;
	max-height: 300px;
	border-radius: 4px;
	overflow: hidden;
}

.pagination {
	margin-top: 40px;
	text-align: center;
}

.pagination :deep(.el-pagination) {
	justify-content: center;
}

.pagination :deep(.el-pagination .el-pagination__total) {
	font-size: 14px;
}

.pagination :deep(.el-pagination .btn-prev),
.pagination :deep(.el-pagination .btn-next) {
	background: #fff;
	border-radius: 8px;
}

.pagination :deep(.el-pagination .el-pager li) {
	background: #fff;
	border-radius: 8px;
	margin: 0 4px;
}

.pagination :deep(.el-pagination .el-pager li.active) {
	background: #1B3E90;
	color: #fff;
}
</style> 