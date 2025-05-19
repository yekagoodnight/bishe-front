<template>
	<div class="addEdit-block" style="background: #f5f7fa; padding: 20px; border-radius: 8px;">
		<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"12px","boxShadow":"0 2px 12px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
			<div slot="header" class="card-header">
				<span style="font-size: 18px; font-weight: 500; color: #333;">{{ type == 'info' ? '详情' : (type == 'add' ? '添加' : '修改') }}</span>
			</div>
			<div v-if="type=='info'" class="info-view">
				<div class="info-row"><span class="info-label">用户账号：</span><span class="info-value">{{ruleForm.yonghuzhanghao}}</span></div>
				<div class="info-row"><span class="info-label">密码：</span><span class="info-value">{{ruleForm.mima}}</span></div>
				<div class="info-row"><span class="info-label">用户姓名：</span><span class="info-value">{{ruleForm.yonghuxingming}}</span></div>
				<div class="info-row"><span class="info-label">性别：</span><span class="info-value">{{ruleForm.xingbie}}</span></div>
				<div class="info-row"><span class="info-label">手机：</span><span class="info-value">{{ruleForm.shouji}}</span></div>
				<div class="info-row"><span class="info-label">身份证：</span><span class="info-value">{{ruleForm.shenfenzheng}}</span></div>
				<div class="info-row"><span class="info-label">头像：</span>
					<span class="info-value">
						<span v-if="ruleForm.touxiang">
							<el-image :src="getAvatarUrl(ruleForm.touxiang)" style="width: 60px; height: 60px; border-radius: 50%; object-fit: cover; margin-left: 10px;"></el-image>
						</span>
						<span v-else>无头像</span>
					</span>
				</div>
				<el-button @click="back()" style="border: none; cursor: pointer; padding: 0 20px; color: #fff; border-radius: 4px; background: #909399; font-size: 14px; height: 40px; margin-top: 24px;">返回</el-button>
			</div>
			<el-form v-else ref="ruleForm" :model="ruleForm" :rules="rules" label-width="100px" label-position="left" style="margin-top: 10px;">
				<el-form-item label="用户账号" prop="yonghuzhanghao">
					<el-input v-model="ruleForm.yonghuzhanghao" placeholder="用户账号" clearable></el-input>
				</el-form-item>
				<el-form-item label="密码" prop="mima">
					<el-input v-model="ruleForm.mima" placeholder="密码" type="password" clearable></el-input>
				</el-form-item>
				<el-form-item label="用户姓名" prop="yonghuxingming">
					<el-input v-model="ruleForm.yonghuxingming" placeholder="用户姓名" clearable></el-input>
				</el-form-item>
				<el-form-item label="性别" prop="xingbie">
					<el-select v-model="ruleForm.xingbie" placeholder="请选择性别">
						<el-option v-for="(item,index) in xingbieOptions" :key="index" :label="item" :value="item"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="手机" prop="shouji">
					<el-input v-model="ruleForm.shouji" placeholder="手机" clearable></el-input>
				</el-form-item>
				<el-form-item label="身份证" prop="shenfenzheng">
					<el-input v-model="ruleForm.shenfenzheng" placeholder="身份证" clearable></el-input>
				</el-form-item>
				<el-form-item label="头像" prop="touxiang">
					<file-upload tip="点击上传头像" action="file/upload" :limit="1" :multiple="false" :fileUrls="ruleForm.touxiang?ruleForm.touxiang:''" @change="touxiangUploadChange"></file-upload>
				</el-form-item>
				<el-form-item>
					<el-button type="primary" @click="onSubmit" style="border: none; cursor: pointer; padding: 0 20px; color: #fff; border-radius: 4px; background: #409EFF; font-size: 14px; height: 40px;">确定</el-button>
					<el-button @click="back()" style="border: 1px solid #dcdfe6; cursor: pointer; padding: 0 20px; color: #333; border-radius: 4px; background: #fff; font-size: 14px; height: 40px;">取消</el-button>
				</el-form-item>
			</el-form>
		</el-card>
	</div>
</template>
<script>
	import { 
		isMobile,
		checkIdCard,
	} from "@/utils/validate";
	export default {
		data() {
			var validateIdCard = (rule, value, callback) => {
				if(!value){
					callback();
				} else if (!checkIdCard(value)) {
					callback(new Error("请输入正确的身份证号码"));
				} else {
					callback();
				}
			};
			var validateMobile = (rule, value, callback) => {
				if(!value){
					callback();
				} else if (!isMobile(value)) {
					callback(new Error("请输入正确的手机号码"));
				} else {
					callback();
				}
			};
			return {
				id: '',
				type: '',
			
			
				ro:{
					yonghuzhanghao : false,
					mima : false,
					yonghuxingming : false,
					xingbie : false,
					shouji : false,
					shenfenzheng : false,
					touxiang : false,
				},
			
				ruleForm: {
					yonghuzhanghao: '',
					mima: '',
					yonghuxingming: '',
					xingbie: '',
					shouji: '',
					shenfenzheng: '',
					touxiang: '',
				},
				xingbieOptions: [],

				rules: {
					yonghuzhanghao: [
						{ required: true, message: '用户账号不能为空', trigger: 'blur' },
					],
					mima: [
						{ required: true, message: '密码不能为空', trigger: 'blur' },
					],
					yonghuxingming: [
						{ required: true, message: '用户姓名不能为空', trigger: 'blur' },
					],
					xingbie: [
						{ required: true, message: '性别不能为空', trigger: 'blur' },
					],
					shouji: [
						{ required: true, message: '手机不能为空', trigger: 'blur' },
						{ validator: validateMobile, trigger: 'blur' },
					],
					shenfenzheng: [
						{ required: true, message: '身份证不能为空', trigger: 'blur' },
						{ validator: validateIdCard, trigger: 'blur' },
					],
					touxiang: [
					],
				},
			};
		},
		props: ["parent"],
		computed: {



		},
		components: {
		},
		created() {
		},
		methods: {
			// 下载
			download(file){
				window.open(`${file}`)
			},
			// 初始化
			init(id,type) {
				if (id) {
					this.id = id;
					this.type = type;
				}
				if(this.type=='info'||this.type=='else'||this.type=='msg'){
					this.info(id);
				}else if(this.type=='logistics'){
					for(let x in this.ro) {
						this.ro[x] = true
					}
					this.logistics=false;
					this.info(id);
				}else if(this.type=='cross'){
					var obj = this.$storage.getObj('crossObj');
					for (var o in obj){
						if(o=='yonghuzhanghao'){
							this.ruleForm.yonghuzhanghao = obj[o];
							this.ro.yonghuzhanghao = true;
							continue;
						}
						if(o=='mima'){
							this.ruleForm.mima = obj[o];
							this.ro.mima = true;
							continue;
						}
						if(o=='yonghuxingming'){
							this.ruleForm.yonghuxingming = obj[o];
							this.ro.yonghuxingming = true;
							continue;
						}
						if(o=='xingbie'){
							this.ruleForm.xingbie = obj[o];
							this.ro.xingbie = true;
							continue;
						}
						if(o=='shouji'){
							this.ruleForm.shouji = obj[o];
							this.ro.shouji = true;
							continue;
						}
						if(o=='shenfenzheng'){
							this.ruleForm.shenfenzheng = obj[o];
							this.ro.shenfenzheng = true;
							continue;
						}
						if(o=='touxiang'){
							this.ruleForm.touxiang = obj[o];
							this.ro.touxiang = true;
							continue;
						}
					}
				}
				// 获取用户信息
				this.$http({
					url: `${this.$storage.get('sessionTable')}/session`,
					method: "get"
				}).then(({ data }) => {
					if (data && data.code === 0) {
						var json = data.data;
					} else {
						this.$message.error(data.msg);
					}
				});
				this.xingbieOptions = "男,女".split(',')
			
			},
			// 多级联动参数

			info(id) {
				this.$http({
					url: `yonghu/info/${id}`,
					method: "get"
				}).then(({ data }) => {
					if (data && data.code === 0) {
						this.ruleForm = data.data;
						//解决前台上传图片后台不显示的问题
						let reg=new RegExp('../../../upload','g')//g代表全部
					} else {
						this.$message.error(data.msg);
					}
				});
			},

			// 提交
			async onSubmit() {
					if(this.ruleForm.touxiang!=null) {
						this.ruleForm.touxiang = this.ruleForm.touxiang.replace(new RegExp(this.$base.url,"g"),"");
					}
					var objcross = this.$storage.getObj('crossObj');
					await this.$refs["ruleForm"].validate(async valid => {
						if (valid) {
							if(this.type=='cross'){
								var statusColumnName = this.$storage.get('statusColumnName');
								var statusColumnValue = this.$storage.get('statusColumnValue');
								if(statusColumnName!='') {
									var obj = this.$storage.getObj('crossObj');
									if(statusColumnName && !statusColumnName.startsWith("[")) {
										for (var o in obj){
											if(o==statusColumnName){
												obj[o] = statusColumnValue;
											}
										}
										var table = this.$storage.get('crossTable');
										await this.$http({
											url: `${table}/update`,
											method: "post",
											data: obj
										}).then(({ data }) => {});
									}
								}
							}
							
							await this.$http({
								url: `yonghu/${!this.ruleForm.id ? "save" : "update"}`,
								method: "post",
								data: this.ruleForm
							}).then(async ({ data }) => {
								if (data && data.code === 0) {
									this.$message({
										message: "操作成功",
										type: "success",
										duration: 1500,
										onClose: () => {
											this.parent.showFlag = true;
											this.parent.addOrUpdateFlag = false;
											this.parent.yonghuCrossAddOrUpdateFlag = false;
											this.parent.search();
											this.parent.contentStyleChange();
										}
									});
								} else {
									this.$message.error(data.msg);
								}
							});
						}
					});
			},
			// 获取uuid
			getUUID () {
				return new Date().getTime();
			},
			// 返回
			back() {
				this.parent.showFlag = true;
				this.parent.addOrUpdateFlag = false;
				this.parent.yonghuCrossAddOrUpdateFlag = false;
				this.parent.contentStyleChange();
			},
			touxiangUploadChange(fileUrls) {
				this.ruleForm.touxiang = fileUrls;
			},
			getAvatarUrl(touxiang) {
				if (!touxiang) {
					return '';
				}
				
				// 多图只取第一个
				let url = touxiang.split(',')[0];
				
				// 处理特定的错误头像路径
				if (url.includes('1747475186509.jpg')) {
					return 'http://localhost:8080/springboot0aqexa96/upload/yonghu_touxiang1.jpg';
				}
				
				// 处理已经是完整URL的情况
				if (url.substring(0, 4) === 'http') {
					return url;
				}
				
				// 处理包含upload路径的情况
				if (url.includes('/upload/')) {
					return 'http://localhost:8080/springboot0aqexa96' + url;
				}
				
				// 默认情况，使用baseUrl拼接
				return this.$base.url + url;
			},
		}
	};
</script>
<style scoped>
	.addEdit-block {
  min-height: calc(100vh - 160px);
	}
	
.el-card {
  margin-bottom: 20px;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1) !important;
	}
	
.el-card .card-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
	}

.add-update-preview .el-form-item {
  padding: 0;
  margin-bottom: 20px;
	}

.add-update-preview .el-input,
.add-update-preview .el-select,
	.add-update-preview .el-date-editor {
		width: 100%;
	}

.add-update-preview .textinfo {
			font-size: 14px;
  padding: 10px 0;
  color: #333;
	}

.add-update-preview .textinfo:before {
  content: '';
  display: block;
  width: 4px;
  height: 14px;
  background: #409EFF;
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
	}
	
.add-update-preview .avatar-box {
  display: flex;
  align-items: center;
  margin-top: 10px;
	}
	
.add-update-preview .avatar-box .el-image {
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  transition: all 0.3s;
			}

.add-update-preview .avatar-box .el-image:hover {
  transform: scale(1.05);
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.15);
			}

.btn {
  display: flex;
  justify-content: center;
  margin-top: 30px;
			}

.btn .el-button {
  transition: all 0.3s;
		}

.btn .el-button:hover {
  transform: translateY(-2px);
  opacity: 0.9;
	}

.info-view {
  margin-bottom: 10px;
}
.info-row {
  display: flex;
  align-items: center;
  margin-bottom: 12px;
}
.info-label {
  width: 100px;
  color: #666;
  font-size: 15px;
  text-align: right;
  flex-shrink: 0;
}
.info-value {
  color: #333;
  font-size: 15px;
  margin-left: 16px;
  word-break: break-all;
}
.el-form-item .el-input,
.el-form-item .el-select {
  width: 100%;
}
.el-form-item .el-input__inner {
  padding-left: 0 !important;
}
::v-deep .el-input__inner, ::v-deep .el-select .el-input__inner {
  padding-left: 0 !important;
}
::v-deep .el-form-item {
  display: flex;
  align-items: center;
  margin-bottom: 22px;
}
::v-deep .el-form-item__label {
  width: 100px;
  text-align: right;
  margin-right: 16px;
  padding: 0;
  line-height: 32px;
}
::v-deep .el-form-item__content {
  margin-left: 0 !important;
  flex: 1;
  min-width: 0;
}
::v-deep .el-input__inner, 
::v-deep .el-select .el-input__inner {
  padding-left: 0 !important;
}
</style>
