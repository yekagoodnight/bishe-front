<template>
	<div>

		<div class="container">
			<el-form class='rgs-form animate__animated animate__bounce' v-if="pageFlag=='register'" ref="registerForm" :model="registerForm" :rules="rules">
				<div class="rgs-form2">
					<div class="title">汽车租赁管理系统</p></div>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="yonghuzhanghao">
						<div class="label" :class="changeRules('yonghuzhanghao')?'required':''">用户账号：</div>
						<el-input v-model="registerForm.yonghuzhanghao"  placeholder="请输入用户账号" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="mima">
						<div class="label" :class="changeRules('mima')?'required':''">密码：</div>
						<el-input v-model="registerForm.mima" type="password" placeholder="请输入密码" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="mima2">
						<div class="label" :class="changeRules('mima')?'required':''">确认密码：</div>
						<el-input v-model="registerForm.mima2" type="password" placeholder="请再次输入密码" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="yonghuxingming">
						<div class="label" :class="changeRules('yonghuxingming')?'required':''">用户姓名：</div>
						<el-input v-model="registerForm.yonghuxingming"  placeholder="请输入用户姓名" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="xingbie">
						<div class="label" :class="changeRules('xingbie')?'required':''">性别：</div>
						<el-select v-model="registerForm.xingbie" placeholder="请选择性别" >
							<el-option
								v-for="(item,index) in yonghuxingbieOptions"
								:key="index"
								:label="item"
								:value="item">
							</el-option>
						</el-select>
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="shouji">
						<div class="label" :class="changeRules('shouji')?'required':''">手机：</div>
						<el-input v-model="registerForm.shouji"  placeholder="请输入手机" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="shenfenzheng">
						<div class="label" :class="changeRules('shenfenzheng')?'required':''">身份证：</div>
						<el-input v-model="registerForm.shenfenzheng"  placeholder="请输入身份证" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='yonghu'" prop="touxiang">
						<div class="label" :class="changeRules('touxiang')?'required':''">头像：</div>
						<file-upload
							tip="点击上传头像"
							action="file/upload"
							:limit="1"
							:multiple="true"
							:fileUrls="registerForm.touxiang?registerForm.touxiang:''"
							@change="yonghutouxiangUploadChange"
						></file-upload>
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="cheshangzhanghao">
						<div class="label" :class="changeRules('cheshangzhanghao')?'required':''">车商账号：</div>
						<el-input v-model="registerForm.cheshangzhanghao"  placeholder="请输入车商账号" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="mima">
						<div class="label" :class="changeRules('mima')?'required':''">密码：</div>
						<el-input v-model="registerForm.mima" type="password" placeholder="请输入密码" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="mima2">
						<div class="label" :class="changeRules('mima')?'required':''">确认密码：</div>
						<el-input v-model="registerForm.mima2" type="password" placeholder="请再次输入密码" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="cheshangxingming">
						<div class="label" :class="changeRules('cheshangxingming')?'required':''">车商姓名：</div>
						<el-input v-model="registerForm.cheshangxingming"  placeholder="请输入车商姓名" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="xingbie">
						<div class="label" :class="changeRules('xingbie')?'required':''">性别：</div>
						<el-select v-model="registerForm.xingbie" placeholder="请选择性别" >
							<el-option
								v-for="(item,index) in cheshangxingbieOptions"
								:key="index"
								:label="item"
								:value="item">
							</el-option>
						</el-select>
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="shouji">
						<div class="label" :class="changeRules('shouji')?'required':''">手机：</div>
						<el-input v-model="registerForm.shouji"  placeholder="请输入手机" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="shenfenzheng">
						<div class="label" :class="changeRules('shenfenzheng')?'required':''">身份证：</div>
						<el-input v-model="registerForm.shenfenzheng"  placeholder="请输入身份证" />
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="zizhiwenjian">
						<div class="label" :class="changeRules('zizhiwenjian')?'required':''">资质文件：</div>
						<file-upload
							tip="点击上传资质文件"
							action="file/upload"
							:limit="1"
							:type="3"
							:multiple="true"
							:fileUrls="registerForm.zizhiwenjian?registerForm.zizhiwenjian:''"
							@change="cheshangzizhiwenjianUploadChange"
						></file-upload>
					</el-form-item>
					<el-form-item class="list-item" v-if="tableName=='cheshang'" prop="touxiang">
						<div class="label" :class="changeRules('touxiang')?'required':''">头像：</div>
						<file-upload
							tip="点击上传头像"
							action="file/upload"
							:limit="1"
							:multiple="true"
							:fileUrls="registerForm.touxiang?registerForm.touxiang:''"
							@change="cheshangtouxiangUploadChange"
						></file-upload>
					</el-form-item>
					<div class="register-btn">
						<div class="register-btn1">
							<el-button class="register_btn" type="primary" @click="submitForm('registerForm')">注册</el-button>
						</div>
						<div class="register-btn2">
							<router-link class="has_btn" to="/login">已有账号，直接登录</router-link>
						</div>
					</div>
				</div>
				<div class="idea1"></div>
				<div class="idea2"></div>
			</el-form>
		</div>
	</div>
</div>
</template>

<script>
	import 'animate.css';

export default {
    //数据集合
    data() {
		return {
            pageFlag : '',
			tableName: '',
			registerForm: {},
			forgetForm: {},
			rules: {},
			requiredRules: {},
            yonghuxingbieOptions: [],
            cheshangxingbieOptions: [],
		}
    },
	mounted() {
		if(this.$route.query.pageFlag=='register'){
			this.tableName = this.$route.query.role;
			if(this.tableName=='yonghu'){
				this.registerForm = {
					yonghuzhanghao: '',
					mima: '',
					mima2: '',
					yonghuxingming: '',
					xingbie: '',
					shouji: '',
					shenfenzheng: '',
					touxiang: '',
				}
			}
			if(this.tableName=='cheshang'){
				this.registerForm = {
					cheshangzhanghao: '',
					mima: '',
					mima2: '',
					cheshangxingming: '',
					xingbie: '',
					shouji: '',
					shenfenzheng: '',
					zizhiwenjian: '',
					touxiang: '',
					sfsh: '',
					shhf: '',
				}
			}
			if ('yonghu' == this.tableName) {
				this.rules.yonghuzhanghao = [{ required: true, message: '请输入用户账号', trigger: 'blur' }];
				this.requiredRules.yonghuzhanghao = [{ required: true, message: '请输入用户账号', trigger: 'blur' }]
			}
			if ('yonghu' == this.tableName) {
				this.rules.mima = [{ required: true, message: '请输入密码', trigger: 'blur' }];
				this.requiredRules.mima = [{ required: true, message: '请输入密码', trigger: 'blur' }]
			}
			if ('yonghu' == this.tableName) {
				this.rules.yonghuxingming = [{ required: true, message: '请输入用户姓名', trigger: 'blur' }];
				this.requiredRules.yonghuxingming = [{ required: true, message: '请输入用户姓名', trigger: 'blur' }]
			}
			this.yonghuxingbieOptions = "男,女".split(',');
			if ('yonghu' == this.tableName) {
				this.rules.xingbie = [{ required: true, message: '请输入性别', trigger: 'blur' }];
				this.requiredRules.xingbie = [{ required: true, message: '请输入性别', trigger: 'blur' }]
			}
			if ('yonghu' == this.tableName) {
				this.rules.shouji = [{ required: true, validator: this.$validate.isMobileNotNull, trigger: 'blur' }];
				this.requiredRules.shouji = [{ required: true, message: '请输入手机', trigger: 'blur' }]
			}
			if ('yonghu' == this.tableName) {
				this.rules.shenfenzheng = [{ required: true, validator: this.$validate.isIdCardNotNull, trigger: 'blur' }];
				this.requiredRules.shenfenzheng = [{ required: true, message: '请输入身份证', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.cheshangzhanghao = [{ required: true, message: '请输入车商账号', trigger: 'blur' }];
				this.requiredRules.cheshangzhanghao = [{ required: true, message: '请输入车商账号', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.mima = [{ required: true, message: '请输入密码', trigger: 'blur' }];
				this.requiredRules.mima = [{ required: true, message: '请输入密码', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.cheshangxingming = [{ required: true, message: '请输入车商姓名', trigger: 'blur' }];
				this.requiredRules.cheshangxingming = [{ required: true, message: '请输入车商姓名', trigger: 'blur' }]
			}
			this.cheshangxingbieOptions = "男,女".split(',');
			if ('cheshang' == this.tableName) {
				this.rules.xingbie = [{ required: true, message: '请输入性别', trigger: 'blur' }];
				this.requiredRules.xingbie = [{ required: true, message: '请输入性别', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.shouji = [{ required: true, validator: this.$validate.isMobileNotNull, trigger: 'blur' }];
				this.requiredRules.shouji = [{ required: true, message: '请输入手机', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.shenfenzheng = [{ required: true, validator: this.$validate.isIdCardNotNull, trigger: 'blur' }];
				this.requiredRules.shenfenzheng = [{ required: true, message: '请输入身份证', trigger: 'blur' }]
			}
			if ('cheshang' == this.tableName) {
				this.rules.zizhiwenjian = [{ required: true, message: '请上传资质文件', trigger: 'blur' }];
				this.requiredRules.zizhiwenjian = [{ required: true, message: '请上传资质文件', trigger: 'blur' }]
			}
		}
	},
    created() {
		this.pageFlag = this.$route.query.pageFlag;
    },
    //方法集合
    methods: {
		changeRules(name){
			if(this.requiredRules[name]){
				return true
			}
			return false
		},
		// 获取uuid
		getUUID () {
			return new Date().getTime();
		},
        // 下二随
		yonghutouxiangUploadChange(fileUrls) {
			this.registerForm.touxiang = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");
		},
		cheshangzizhiwenjianUploadChange(fileUrls) {
			this.registerForm.zizhiwenjian = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");
		},
		cheshangtouxiangUploadChange(fileUrls) {
			this.registerForm.touxiang = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");
		},

		// 多级联动参数


		submitForm(formName) {
			this.$refs[formName].validate((valid) => {
				if (valid) {
					var url=this.tableName+"/register";
					if(`yonghu` == this.tableName && this.registerForm.mima!=this.registerForm.mima2) {
						this.$message.error(`两次密码输入不一致`);
						return
					}
					if(this.tableName=='cheshang'){
						this.registerForm.sfsh = '待审核'
					}
					if(`cheshang` == this.tableName && this.registerForm.mima!=this.registerForm.mima2) {
						this.$message.error(`两次密码输入不一致`);
						return
					}
					this.$http.post(url, this.registerForm).then(res => {
						if (res.data.code === 0) {
							this.$message({
								message: '注册成功',
								type: 'success',
								duration: 1500,
								onClose: () => {
									this.$router.push('/login');
								}
							});
						} else {
							this.$message.error(res.data.msg);
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
	.container {
		background: url(http://codegen.caihongy.cn/20250124/cc37cc4ce1874ed79b5758a708712da2.png) center center/100% 100%;
		display: flex;
		width: 100%;
		min-height: 100vh;
		justify-content: flex-start;
		align-items: center;
		position: relative;
		background: url(http://codegen.caihongy.cn/20250124/cc37cc4ce1874ed79b5758a708712da2.png) center center/100% 100%;
		.rgs-form {
			border-radius: 10px;
			padding: 0 0 20px;
			box-shadow: 0px 5px 10px 0px rgba(0, 0, 0, 0.25);
			max-height: 80vh;
			margin: 0 0 0 15%;
			z-index: 1;
			overflow: auto;
			background: #fff;
			width: 25%;
			height: auto;
			.rgs-form2 {
				width: 100%;
				.title {
					border-radius: 10px 10px 30px 30px;
					margin: 0 0 30px 0;
					color: #fff;
					background: #25489C;
					font-weight: bold;
					width: 100%;
					font-size: 20px;
					line-height: 60px;
					text-align: center;
				}
				.subtitle {
					margin: 0 0 10px 0;
					text-shadow: 4px 4px 2px rgba(64, 158, 255, .5);
					color: rgba(64, 158, 255, 1);
					width: 100%;
					font-size: 20px;
					line-height: 44px;
					text-align: center;
				}
				.list-item {
					box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
					padding: 0 0 0 10px;
					margin: 0 auto 30px;
					display: flex;
					width: 75%;
					border-color: rgb(180, 166, 129);
					border-width: 0 0 2px;
					border-style: solid;
					/deep/.el-form-item__content {
						display: flex;
						align-items: flex-start;
						.label {
							color: #B4A681;
							width: 80px;
							font-size: 14px;
							line-height: 50px;
						}
						
						.required {
							position: relative;
						}
						.required::after{
							color: red;
							left: -10px;
							position: absolute;
							content: "*";
						}
						.el-input {
							width: calc(100% - 80px);
						}
						.el-input .el-input__inner {
							border: 0px solid rgba(64, 158, 255, 1);
							padding: 0 10px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: 0px solid #efefef;
							color: #B4A681;
							width: 100%;
							font-size: 14px;
							outline-offset: 0;
							height: 50px;
						}
						.el-input .el-input__inner:focus {
							border: 0px solid rgba(64, 158, 255, 1);
							padding: 0 10px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: 0px solid #efefef;
							color: #B4A681;
							font-weight: bold;
							width: 100%;
							font-size: 14px;
							outline-offset: 0;
							height: 50px;
						}
						.el-input-number {
							width: calc(100% - 80px);
						}
						.el-input-number .el-input__inner {
							text-align: left;
							border: 0px solid rgba(64, 158, 255, 1);
							padding: 0 10px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: 0px solid #efefef;
							color: #B4A681;
							width: 100%;
							font-size: 14px;
							outline-offset: 0;
							height: 50px;
						}
						.el-input-number .el-input-number__decrease {
							display: none;
						}
						.el-input-number .el-input-number__increase {
							display: none;
						}
						.el-select {
							width: calc(100% - 80px);
						}
						.el-select .el-input__inner {
							border: 0;
							border-radius: 0;
							padding: 0 10px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: none;
							color: #B4A681;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.el-select .el-input__inner:focus {
							border: 0;
							border-radius: 0;
							padding: 0 10px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: none;
							color: #B4A681;
							font-weight: bold;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.el-date-editor {
							width: calc(100% - 80px);
						}
						.el-date-editor .el-input__inner {
							border: 0;
							border-radius: 0;
							padding: 0 10px 0 30px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: none;
							color: #B4A681;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.el-date-editor .el-input__inner:focus {
							border: 0;
							border-radius: 0;
							padding: 0 10px 0 30px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							outline: none;
							color: #B4A681;
							font-weight: bold;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.el-upload--picture-card {
							background: transparent;
							border: 0;
							border-radius: 0;
							width: auto;
							height: auto;
							line-height: initial;
							vertical-align: middle;
						}
						.upload .upload-img {
							border: 1px dashed #B4A681;
							cursor: pointer;
							border-radius: 8px;
							margin: 0 0 10px;
							color: #B4A681;
							width: 80px;
							font-size: 32px;
							line-height: 80px;
							text-align: center;
							height: 80px;
						}
						.el-upload-list .el-upload-list__item {
							border: 1px dashed #B4A681;
							cursor: pointer;
							border-radius: 8px;
							margin: 0 0 10px;
							color: #B4A681;
							width: 80px;
							font-size: 32px;
							line-height: 80px;
							text-align: center;
							height: 80px;
							font-size: 14px;
							line-height: 1.8;
						}
						.el-upload .el-icon-plus {
							border: 1px dashed #B4A681;
							cursor: pointer;
							border-radius: 8px;
							margin: 0 0 10px;
							color: #B4A681;
							width: 80px;
							font-size: 32px;
							line-height: 80px;
							text-align: center;
							height: 80px;
						}
						.el-upload__tip {
							color: #838fa1;
							display: none;
						}
						.emailInput {
							border: 0;
							border-radius: 0;
							padding: 0 12px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							margin: 0;
							outline: none;
							color: #B4A681;
							background: #fff;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.emailInput:focus {
							border: 0;
							border-radius: 0;
							padding: 0 12px;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							margin: 0;
							outline: none;
							color: #B4A681;
							background: #fff;
							width: 100%;
							font-size: 14px;
							height: 50px;
						}
						.el-btn {
							border: 0;
							cursor: pointer;
							border-radius: 0;
							padding: 0;
							box-shadow: 0 0 0px rgba(64, 158, 255, .5);
							margin: 0;
							outline: none;
							color: #fff;
							background: #25489C;
							width: 80px;
							font-size: 12px;
							height: 50px;
						}
						.el-btn:hover {
							opacity: 0.5;
						}
						
						.el-input__inner::placeholder {
							color: #CDCDCD;
							font-size: 14px;
						}
						input::placeholder {
							color: #CDCDCD;
							font-size: 14px;
						}
						.editor {
							margin: 0 0 10px;
							width: 100%;
							height: auto;
						}
					}
				}
				.register-btn {
					width: 100%;
				}
				.register-btn1 {
					width: 100%;
				}
				.register-btn2 {
					width: 100%;
				}
				.register_btn {
					border: 0;
					cursor: pointer;
					padding: 0 10px;
					margin: 20px auto 5px;
					color: #fff;
					display: block;
					font-size: 18px;
					border-radius: 0;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					background: #25489C;
					width: 75%;
					height: 50px;
				}
				.register_btn:hover {
					opacity: 0.5;
				}
				.has_btn {
					cursor: pointer;
					padding: 0;
					color: #AFAFAF;
					display: block;
					text-decoration: none;
					font-size: 12px;
					line-height: 3;
					text-align: center;
				}
				.has_btn:hover {
					opacity: 0.5;
				}
			}
			.idea1 {
				background: red;
				display: none;
				width: 100%;
				height: 40px;
			}
			.idea2 {
				background: blue;
				display: none;
				width: 100%;
				height: 40px;
			}
		}
	}
	
	::-webkit-scrollbar {
		display: none;
	}
</style>
