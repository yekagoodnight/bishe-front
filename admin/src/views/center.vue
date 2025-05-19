<template>
	<div :style='{"width":"100%","padding":"20px 30px","fontSize":"15px"}'>
		<el-form
			:style='{"padding":"40px 30px","borderColor":"#eee","borderStyle":"solid","borderWidth":"0px 0 0","background":"none","maxWidth":"600px","margin":"0 auto","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","borderRadius":"12px"}'
			class="add-update-preview"
			ref="ruleForm"
			:model="ruleForm"
			label-width="180px"
		>
			<div class="form-header" style="margin-bottom: 25px; padding-bottom: 15px; border-bottom: 1px solid #ebeef5;">
				<h3 style="margin: 0; font-size: 18px; color: #333; font-weight: 500;">个人信息管理</h3>
				<p style="margin: 8px 0 0; font-size: 14px; color: #909399;">查看并编辑您的个人信息</p>
			</div>
			
			<el-form-item v-if="!editMode" label="">
				<el-button class="edit-btn" type="primary" @click="startEdit">
					<i class="el-icon-edit" style="margin-right: 5px;"></i>编辑信息
				</el-button>
			</el-form-item>
			
			<!-- 用户信息表单 - 查看模式 -->
			<template v-if="!editMode">
				<el-form-item v-if="flag=='yonghu'" label="用户账号" prop="yonghuzhanghao">
					<el-input v-model="ruleForm.yonghuzhanghao" disabled placeholder="用户账号" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="用户姓名" prop="yonghuxingming">
					<el-input v-model="ruleForm.yonghuxingming" disabled placeholder="用户姓名" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="性别" prop="xingbie">
					<el-input v-model="ruleForm.xingbie" disabled placeholder="性别" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="手机" prop="shouji">
					<el-input v-model="ruleForm.shouji" disabled placeholder="手机" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="身份证" prop="shenfenzheng">
					<el-input v-model="ruleForm.shenfenzheng" disabled placeholder="身份证" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="头像" prop="touxiang">
					<el-image 
						v-if="ruleForm.touxiang" 
						:src="getAvatarUrl(ruleForm.touxiang)" 
						style="width: 100px; height: 100px; border-radius: 50%; object-fit: cover; border: 3px solid #fff; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
						:preview-src-list="[getAvatarUrl(ruleForm.touxiang)]">
					</el-image>
					<div v-else style="width: 100px; height: 100px; border-radius: 50%; background-color: #f5f7fa; display: flex; justify-content: center; align-items: center; color: #909399;">
						<i class="el-icon-user" style="font-size: 36px;"></i>
					</div>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="车商账号" prop="cheshangzhanghao">
					<el-input v-model="ruleForm.cheshangzhanghao" disabled placeholder="车商账号" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="车商姓名" prop="cheshangxingming">
					<el-input v-model="ruleForm.cheshangxingming" disabled placeholder="车商姓名" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="性别" prop="xingbie">
					<el-input v-model="ruleForm.xingbie" disabled placeholder="性别" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="手机" prop="shouji">
					<el-input v-model="ruleForm.shouji" disabled placeholder="手机" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="身份证" prop="shenfenzheng">
					<el-input v-model="ruleForm.shenfenzheng" disabled placeholder="身份证" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="资质文件" prop="zizhiwenjian">
					<el-button type="text" @click="downloadFile(ruleForm.zizhiwenjian)" v-if="ruleForm.zizhiwenjian">
						<i class="el-icon-download" style="margin-right: 5px;"></i>查看资质文件
					</el-button>
					<span v-else>暂无资质文件</span>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="头像" prop="touxiang">
					<el-image 
						v-if="ruleForm.touxiang" 
						:src="getAvatarUrl(ruleForm.touxiang)" 
						style="width: 100px; height: 100px; border-radius: 50%; object-fit: cover; border: 3px solid #fff; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
						:preview-src-list="[getAvatarUrl(ruleForm.touxiang)]">
					</el-image>
					<div v-else style="width: 100px; height: 100px; border-radius: 50%; background-color: #f5f7fa; display: flex; justify-content: center; align-items: center; color: #909399;">
						<i class="el-icon-user" style="font-size: 36px;"></i>
					</div>
				</el-form-item>
				<el-form-item v-if="flag=='users'" label="用户名" prop="username">
					<el-input v-model="ruleForm.username" disabled placeholder="用户名"></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='users'" label="头像" prop="image">
					<el-image 
						v-if="ruleForm.image" 
						:src="getAvatarUrl(ruleForm.image)" 
						style="width: 100px; height: 100px; border-radius: 50%; object-fit: cover; border: 3px solid #fff; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
						:preview-src-list="[getAvatarUrl(ruleForm.image)]">
					</el-image>
					<div v-else style="width: 100px; height: 100px; border-radius: 50%; background-color: #f5f7fa; display: flex; justify-content: center; align-items: center; color: #909399;">
						<i class="el-icon-user" style="font-size: 36px;"></i>
					</div>
				</el-form-item>
			</template>
			
			<!-- 用户信息表单 - 编辑模式 -->
			<template v-else>
				<el-form-item v-if="flag=='yonghu'" label="用户账号" prop="yonghuzhanghao">
					<el-input v-model="ruleForm.yonghuzhanghao" disabled placeholder="用户账号" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="用户姓名" prop="yonghuxingming">
					<el-input v-model="ruleForm.yonghuxingming" placeholder="用户姓名" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="性别" prop="xingbie">
					<el-select v-model="ruleForm.xingbie" placeholder="请选择性别">
						<el-option
							v-for="(item,index) in yonghuxingbieOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="手机" prop="shouji">
					<el-input v-model="ruleForm.shouji" placeholder="手机" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="身份证" prop="shenfenzheng">
					<el-input v-model="ruleForm.shenfenzheng" placeholder="身份证" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='yonghu'" label="头像" prop="touxiang">
					<file-upload
						tip="点击上传头像"
						action="file/upload"
						:limit="3"
						:multiple="true"
						:fileUrls="ruleForm.touxiang?ruleForm.touxiang:''"
						@change="yonghutouxiangUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="车商账号" prop="cheshangzhanghao">
					<el-input v-model="ruleForm.cheshangzhanghao" disabled placeholder="车商账号" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="车商姓名" prop="cheshangxingming">
					<el-input v-model="ruleForm.cheshangxingming" placeholder="车商姓名" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="性别" prop="xingbie">
					<el-select v-model="ruleForm.xingbie" placeholder="请选择性别">
						<el-option
							v-for="(item,index) in cheshangxingbieOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="手机" prop="shouji">
					<el-input v-model="ruleForm.shouji" placeholder="手机" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="身份证" prop="shenfenzheng">
					<el-input v-model="ruleForm.shenfenzheng" placeholder="身份证" clearable></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="资质文件" prop="zizhiwenjian">
					<file-upload
						tip="点击上传资质文件"
						action="file/upload"
						:limit="3"
						:type="3"
						:multiple="true"
						:fileUrls="ruleForm.zizhiwenjian?ruleForm.zizhiwenjian:''"
						@change="cheshangzizhiwenjianUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item v-if="flag=='cheshang'" label="头像" prop="touxiang">
					<file-upload
						tip="点击上传头像"
						action="file/upload"
						:limit="3"
						:multiple="true"
						:fileUrls="ruleForm.touxiang?ruleForm.touxiang:''"
						@change="cheshangtouxiangUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item v-if="flag=='users'" label="用户名" prop="username">
					<el-input v-model="ruleForm.username" placeholder="用户名"></el-input>
				</el-form-item>
				<el-form-item v-if="flag=='users'" label="头像" prop="image">
					<file-upload
						tip="点击上传头像"
						action="file/upload"
						:limit="1"
						:multiple="false"
						:fileUrls="ruleForm.image?ruleForm.image:''"
						@change="usersimageUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item class="form-buttons">
					<el-button class="btn3" type="primary" @click="saveEdit">
						<i class="el-icon-check" style="margin-right: 5px;"></i>确定
					</el-button>
					<el-button class="cancel-btn" @click="cancelEdit">
						<i class="el-icon-close" style="margin-right: 5px;"></i>取消
					</el-button>
				</el-form-item>
			</template>
		</el-form>
	</div>
</template>
<script>
// 校验引入
import { 
	isMobile,
	checkIdCard,
} from "@/utils/validate";

export default {
	data() {
		return {
			ruleForm: {},
			flag: '',
			usersFlag: false,
			yonghuxingbieOptions: [],
			cheshangxingbieOptions: [],
			editMode: false,
			backupRuleForm: {},
		};
	},
	mounted() {
		var table = this.$storage.get("sessionTable");
		this.flag = table;
		this.loadUserData();
		this.yonghuxingbieOptions = "男,女".split(',')
		this.cheshangxingbieOptions = "男,女".split(',')
	},
	methods: {
		loadUserData() {
		this.$http({
			url: `${this.$storage.get("sessionTable")}/session`,
			method: "get"
		}).then(({ data }) => {
			if (data && data.code === 0) {
				this.ruleForm = data.data;
					// 创建备份，用于取消编辑时恢复
					this.backupRuleForm = JSON.parse(JSON.stringify(this.ruleForm));
			} else {
				this.$message.error(data.msg);
			}
		});
		},
		startEdit() {
			// 切换到编辑模式前先创建一个数据备份
		this.backupRuleForm = JSON.parse(JSON.stringify(this.ruleForm));
			this.editMode = true;
	},
		yonghutouxiangUploadChange(fileUrls) {
			this.ruleForm.touxiang = fileUrls;
		},
		cheshangzizhiwenjianUploadChange(fileUrls) {
			this.ruleForm.zizhiwenjian = fileUrls;
		},
		cheshangtouxiangUploadChange(fileUrls) {
			this.ruleForm.touxiang = fileUrls;
		},
		usersimageUploadChange(fileUrls) {
			this.ruleForm.image = fileUrls;
		},
		downloadFile(url) {
			if (!url) return;
			window.open(this.getFileUrl(url));
		},
		getFileUrl(url) {
			if (!url) return '';
			// 多文件时只取第一个
			let fileUrl = url.split(',')[0];
			if (fileUrl.startsWith('http')) {
				return fileUrl;
			}
			return this.$base.url + fileUrl;
		},
		getAvatarUrl(url) {
			if (!url) {
				return '';
			}
			
			// 多图只取第一个
			let avatarUrl = url.split(',')[0];
			
			// 处理特定的错误头像路径
			if (avatarUrl.includes('1747475186509.jpg')) {
				return 'http://localhost:8080/springboot0aqexa96/upload/yonghu_touxiang1.jpg';
			}
			
			// 处理已经是完整URL的情况
			if (avatarUrl.substring(0, 4) === 'http') {
				return avatarUrl;
			}
			
			// 处理包含upload路径的情况
			if (avatarUrl.includes('/upload/')) {
				return 'http://localhost:8080/springboot0aqexa96' + avatarUrl;
			}
			
			// 默认情况，使用baseUrl拼接
			return this.$base.url + avatarUrl;
		},
		onUpdateHandler() {
			console.log("正在验证表单数据...", this.ruleForm);
			
			// 验证用户信息
			if('yonghu'==this.flag){
				if(!this.ruleForm.yonghuzhanghao){
				this.$message.error('用户账号不能为空');
					return false;
			}

				// 不检查密码，因为session中可能不包含密码字段
				// if(!this.ruleForm.mima){
				//  this.$message.error('密码不能为空');
				//  return false;
				//}

				if(!this.ruleForm.yonghuxingming){
				this.$message.error('用户姓名不能为空');
					return false;
			}

				if(!this.ruleForm.xingbie){
				this.$message.error('性别不能为空');
					return false;
			}

				if(!this.ruleForm.shouji){
				this.$message.error('手机不能为空');
					return false;
			}

				if(this.ruleForm.shouji && !isMobile(this.ruleForm.shouji)){
				this.$message.error(`手机应输入手机格式`);
					return false;
			}
				
				if(!this.ruleForm.shenfenzheng){
				this.$message.error('身份证不能为空');
					return false;
			}

				if(this.ruleForm.shenfenzheng && !checkIdCard(this.ruleForm.shenfenzheng)){
				this.$message.error(`身份证应输入身份证格式`);
					return false;
			}

				if(this.ruleForm.touxiang) {
				this.ruleForm.touxiang = this.ruleForm.touxiang.replace(new RegExp(this.$base.url,"g"),"");
			}
			}
			
			// 验证车商信息
			if('cheshang'==this.flag){
				if(!this.ruleForm.cheshangzhanghao){
				this.$message.error('车商账号不能为空');
					return false;
			}

				// 不检查密码，因为session中可能不包含密码字段
				// if(!this.ruleForm.mima){
				//  this.$message.error('密码不能为空');
				//  return false;
				//}

				if(!this.ruleForm.cheshangxingming){
				this.$message.error('车商姓名不能为空');
					return false;
			}

				if(!this.ruleForm.xingbie){
				this.$message.error('性别不能为空');
					return false;
			}

				if(!this.ruleForm.shouji){
				this.$message.error('手机不能为空');
					return false;
			}

				if(this.ruleForm.shouji && !isMobile(this.ruleForm.shouji)){
				this.$message.error(`手机应输入手机格式`);
					return false;
			}
				
				if(!this.ruleForm.shenfenzheng){
				this.$message.error('身份证不能为空');
					return false;
			}

				if(this.ruleForm.shenfenzheng && !checkIdCard(this.ruleForm.shenfenzheng)){
				this.$message.error(`身份证应输入身份证格式`);
					return false;
				}

				if(this.ruleForm.zizhiwenjian) {
				this.ruleForm.zizhiwenjian = this.ruleForm.zizhiwenjian.replace(new RegExp(this.$base.url,"g"),"");
			}

				if(this.ruleForm.touxiang) {
				this.ruleForm.touxiang = this.ruleForm.touxiang.replace(new RegExp(this.$base.url,"g"),"");
			}
			}
			
			// 验证管理员信息
			if('users'==this.flag){
				if(this.ruleForm.username && this.ruleForm.username.trim().length < 1) {
				this.$message.error(`用户名不能为空`);
					return false;	
			}
				
				if(this.ruleForm.image) {
					this.ruleForm.image = this.ruleForm.image.replace(new RegExp(this.$base.url,"g"),"");
			}
			}
			
			console.log("表单验证通过，准备提交数据:", this.ruleForm);
			return true;
		},
		saveEdit() {
			console.log("点击了保存按钮");
			if(!this.onUpdateHandler()) {
				console.log("表单验证失败，不执行保存");
				return;
			}
			
			// 创建一个新对象，只包含需要更新的字段
			const updateData = {};
			// 复制所有字段，除了密码相关字段
			for (const key in this.ruleForm) {
				if (key !== 'mima' && key !== 'password') {
					updateData[key] = this.ruleForm[key];
				}
			}
			
			console.log("即将发送更新请求:", updateData);
			this.$http({
				url: `${this.$storage.get("sessionTable")}/update`,
				method: "post",
				data: updateData
			}).then(({ data }) => {
				console.log("更新请求返回结果:", data);
				if (data && data.code === 0) {
					this.$message({
						message: "修改信息成功",
						type: "success",
						duration: 1500,
						onClose: () => {
							if(this.flag=='users'){
								this.$storage.set('headportrait',this.ruleForm.image)
							}
							this.editMode = false;
							// 更新备份
							this.backupRuleForm = JSON.parse(JSON.stringify(this.ruleForm));
							// 刷新数据
							this.loadUserData();
						}
					});
				} else {
					this.$message.error(data.msg || "更新失败，请稍后重试");
				}
			}).catch(err => {
				console.error("更新请求发生错误:", err);
				this.$message.error("更新失败，请检查网络连接");
			});
		},
		cancelEdit() {
			// 从备份中恢复数据
			this.ruleForm = JSON.parse(JSON.stringify(this.backupRuleForm));
			this.editMode = false;
		}
	}
};
</script>
<style lang="css" scoped>
	.el-date-editor.el-input {
		width: auto;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
				width: 180px;
				text-align: right;
				margin-right: 16px;
				padding: 0;
				line-height: 40px;
				font-size: 15px;
				color: #6e6e6e;
				font-weight: 500;
			}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
				margin-left: 0 !important;
				flex: 1;
				min-width: 0;
			}
	
	.add-update-preview .el-input,
	.add-update-preview .el-select {
				width: 100%;
			}
	
	.add-update-preview .el-input /deep/ .el-input__inner,
	.add-update-preview .el-select /deep/ .el-input__inner {
				width: 100%;
				font-size: 15px;
				height: 40px;
				padding-left: 0 !important;
				border-radius: 4px;
				border: 1px solid #dcdfe6;
				transition: all 0.3s;
			}
	
	.add-update-preview .el-input /deep/ .el-input__inner:focus,
	.add-update-preview .el-select /deep/ .el-input__inner:focus {
		border-color: #409EFF;
		box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
			}
	
	.add-update-preview /deep/ .el-upload--picture-card {
				background: transparent;
				border: 0;
				border-radius: 0;
				width: auto;
				height: auto;
				line-height: initial;
				vertical-align: middle;
			}
	
	.add-update-preview /deep/ .el-upload-list .el-upload-list__item {
				border: 1px solid #E8E8E8;
				cursor: pointer;
				border-radius: 0px;
				color: #666;
				background: #fff;
				width: 90px;
				font-size: 24px;
				line-height: 60px;
				text-align: center;
				height: 60px;
			}
	
	.add-update-preview /deep/ .el-upload .el-icon-plus {
				border: 1px solid #E8E8E8;
				cursor: pointer;
				border-radius: 0px;
				color: #666;
				background: #fff;
				width: 90px;
				font-size: 24px;
				line-height: 60px;
				text-align: center;
				height: 60px;
			}
	
	.add-update-preview .el-textarea /deep/ .el-textarea__inner {
				border: 1px solid #E8E8E8;
				border-radius: 0px;
				padding: 12px;
				color: #666;
				background: #fff;
				width: auto;
				font-size: 15px;
				min-width: 100%;
				height: 120px;
			}
	
	.el-form-item .el-button[type="primary"] {
				background: linear-gradient(90deg, #1B3E90 0%, #25489C 100%);
				color: #fff;
				border: none;
				border-radius: 22px;
				box-shadow: 0 2px 8px rgba(27, 62, 144, 0.12);
				font-size: 16px;
				height: 44px;
				min-width: 120px;
				padding: 0 28px;
				transition: all 0.2s;
				margin-right: 16px;
			}
	
	.el-form-item .el-button[type="primary"]:hover {
				background: linear-gradient(90deg, #25489C 0%, #1B3E90 100%);
				box-shadow: 0 4px 16px rgba(27, 62, 144, 0.18);
				transform: translateY(-2px) scale(1.04);
			}
	
	.el-form-item .el-button.btn3[type="primary"] {
				background: linear-gradient(90deg, #32c67a 0%, #43e97b 100%);
				color: #fff;
				border: none;
				border-radius: 22px;
				box-shadow: 0 2px 8px rgba(50, 198, 122, 0.12);
				font-size: 16px;
				height: 44px;
				min-width: 120px;
				padding: 0 28px;
				transition: all 0.2s;
				margin-right: 16px;
			}
	
	.el-form-item .el-button.btn3[type="primary"]:hover {
				background: linear-gradient(90deg, #43e97b 0%, #32c67a 100%);
				box-shadow: 0 4px 16px rgba(50, 198, 122, 0.18);
				transform: translateY(-2px) scale(1.04);
			}
	
	.el-form-item .el-button.cancel-btn {
				background: #f0f2f5;
				color: #888;
				border: none;
				border-radius: 22px;
				font-size: 16px;
				height: 44px;
				min-width: 120px;
				padding: 0 28px;
				margin-right: 16px;
				transition: all 0.2s;
			}
	
	.el-form-item .el-button.cancel-btn:hover {
				background: #e0e3e8;
				color: #555;
				transform: translateY(-2px) scale(1.04);
			}
	
	.editor>.avatar-uploader {
		line-height: 0;
		height: 0;
	}
	
	/* 隐藏el-upload的图片列表缩略图 */
	::v-deep .el-upload-list--picture-card {
				display: none !important;
			}
	
	.add-update-preview /deep/ .edit-btn {
		background: linear-gradient(90deg, #1B3E90 0%, #25489C 100%);
		color: #fff;
		border: none;
		border-radius: 22px;
		box-shadow: 0 2px 8px rgba(27, 62, 144, 0.12);
		font-size: 16px;
		height: 44px;
		min-width: 120px;
		padding: 0 28px;
		transition: all 0.2s;
		margin-right: 16px;
	}
	.add-update-preview /deep/ .edit-btn:hover {
		background: linear-gradient(90deg, #25489C 0%, #1B3E90 100%);
		box-shadow: 0 4px 16px rgba(27, 62, 144, 0.18);
		transform: translateY(-2px) scale(1.04);
	}
	.add-update-preview /deep/ .btn3[type="primary"] {
		background: linear-gradient(90deg, #32c67a 0%, #43e97b 100%);
		color: #fff;
		border: none;
		border-radius: 22px;
		box-shadow: 0 2px 8px rgba(50, 198, 122, 0.12);
		font-size: 16px;
		height: 44px;
		min-width: 120px;
		padding: 0 28px;
		transition: all 0.2s;
		margin-right: 16px;
	}
	.add-update-preview /deep/ .btn3[type="primary"]:hover {
		background: linear-gradient(90deg, #43e97b 0%, #32c67a 100%);
		box-shadow: 0 4px 16px rgba(50, 198, 122, 0.18);
		transform: translateY(-2px) scale(1.04);
	}
	.add-update-preview /deep/ .cancel-btn {
		background: #f0f2f5;
		color: #888;
		border: none;
		border-radius: 22px;
		font-size: 16px;
		height: 44px;
		min-width: 120px;
		padding: 0 28px;
		margin-right: 16px;
		transition: all 0.2s;
	}
	.add-update-preview /deep/ .cancel-btn:hover {
		background: #e0e3e8;
		color: #555;
		transform: translateY(-2px) scale(1.04);
	}
	.add-update-preview /deep/ .el-upload {
		display: inline-block;
	}
	.add-update-preview /deep/ .el-upload .el-button,
	.add-update-preview /deep/ .el-upload input[type="file"]::-webkit-file-upload-button {
		background: linear-gradient(90deg, #1B3E90 0%, #25489C 100%);
		color: #fff;
		border: none;
		border-radius: 22px;
		box-shadow: 0 2px 8px rgba(27, 62, 144, 0.12);
		font-size: 15px;
		height: 40px;
		min-width: 110px;
		padding: 0 22px;
		transition: all 0.2s;
		margin-right: 12px;
		cursor: pointer;
	}
	.add-update-preview /deep/ .el-upload .el-button:hover,
	.add-update-preview /deep/ .el-upload input[type="file"]::-webkit-file-upload-button:hover {
		background: linear-gradient(90deg, #25489C 0%, #1B3E90 100%);
		box-shadow: 0 4px 16px rgba(27, 62, 144, 0.18);
		transform: translateY(-2px) scale(1.04);
	}
	.add-update-preview /deep/ .el-upload-list__item-name {
		color: #1B3E90;
		font-weight: 500;
		font-size: 14px;
		margin-left: 8px;
	}
	
	/* 表单标题样式 */
	.form-header {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		position: relative;
		padding-bottom: 20px;
		margin-bottom: 30px;
		text-align: center;
	}
	
	.form-header:after {
		content: '';
		position: absolute;
		bottom: 0;
		left: 50%;
		transform: translateX(-50%);
		width: 60px;
		height: 3px;
		background: linear-gradient(90deg, #1B3E90, #32c67a);
		border-radius: 3px;
	}
	
	.form-buttons {
		display: flex;
		justify-content: center;
		margin-top: 30px;
	}
</style>
