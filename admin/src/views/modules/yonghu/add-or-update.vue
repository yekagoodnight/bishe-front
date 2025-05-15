<template>
	<div class="addEdit-block" style="background: #f5f7fa; padding: 20px; border-radius: 8px;">
		<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"12px","boxShadow":"0 2px 12px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
			<div slot="header" class="card-header">
				<span style="font-size: 18px; font-weight: 500; color: #333;">{{ type == 'info' ? '详情' : (type == 'add' ? '添加' : '修改') }}</span>
			</div>
		<el-form
			class="add-update-preview"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
				label-width="120px"
				:style='{"padding":"0","margin":"0 auto"}'
		>
				<el-row :gutter="20">
					<el-col :span="12">
						<el-form-item class="input" :label="type!='info'?'用户账号':''" prop="yonghuzhanghao" :class="type=='info'?'textinfo':''">
							<el-input v-if="type!='info'" v-model="ruleForm.yonghuzhanghao" placeholder="用户账号" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.yonghuzhanghao"></el-input>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">用户账号:</div>
								<div style="padding: 0 12px;">{{ruleForm.yonghuzhanghao}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="12">
						<el-form-item class="input" :label="type!='info'?'密码':''" prop="mima" :class="type=='info'?'textinfo':''">
							<el-input v-if="type!='info'" v-model="ruleForm.mima" placeholder="密码" :type="true ? 'password' : 'text'" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.mima"></el-input>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">密码:</div>
								<div style="padding: 0 12px;">{{ruleForm.mima}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="12">
						<el-form-item class="input" :label="type!='info'?'用户姓名':''" prop="yonghuxingming" :class="type=='info'?'textinfo':''">
							<el-input v-if="type!='info'" v-model="ruleForm.yonghuxingming" placeholder="用户姓名" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.yonghuxingming"></el-input>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">用户姓名:</div>
								<div style="padding: 0 12px;">{{ruleForm.yonghuxingming}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="12">
						<el-form-item class="select" :label="type!='info'?'性别':''" prop="xingbie" :class="type=='info'?'textinfo':''">
							<el-select v-if="type!='info'" v-model="ruleForm.xingbie" placeholder="请选择性别" :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :disabled="ro.xingbie">
						<el-option
							v-for="(item,index) in xingbieOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">性别:</div>
								<div style="padding: 0 12px;">{{ruleForm.xingbie}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="12">
						<el-form-item class="input" :label="type!='info'?'手机':''" prop="shouji" :class="type=='info'?'textinfo':''">
							<el-input v-if="type!='info'" v-model="ruleForm.shouji" placeholder="手机" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.shouji"></el-input>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">手机:</div>
								<div style="padding: 0 12px;">{{ruleForm.shouji}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="12">
						<el-form-item class="input" :label="type!='info'?'身份证':''" prop="shenfenzheng" :class="type=='info'?'textinfo':''">
							<el-input v-if="type!='info'" v-model="ruleForm.shenfenzheng" placeholder="身份证" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.shenfenzheng"></el-input>
							<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
								<div style="font-weight: bold; margin-bottom: 5px;">身份证:</div>
								<div style="padding: 0 12px;">{{ruleForm.shenfenzheng}}</div>
							</div>
				</el-form-item>
					</el-col>
					<el-col :span="24">
						<el-form-item class="upload" :label="type!='info'?'头像':''" prop="touxiang" :class="type=='info'?'textinfo':''">
							<template v-if="type!='info' && !ro.touxiang">
					<file-upload
						tip="点击上传头像"
						action="file/upload"
									:limit="1"
									:multiple="false"
						:fileUrls="ruleForm.touxiang?ruleForm.touxiang:''"
						@change="touxiangUploadChange"
									:style='{"width":"100%","margin":"0 0 20px"}'
					></file-upload>
							</template>
							<template v-if="type=='info'">
								<div style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">头像:</div>
									<div class="avatar-box" v-if="ruleForm.touxiang" style="display: flex; align-items: center;">
										<el-image 
											:src="ruleForm.touxiang && ruleForm.touxiang.substring(0,4) == 'http' ? 
												(ruleForm.touxiang.split(',w').length > 1 ? ruleForm.touxiang : ruleForm.touxiang.split(',')[0]) : 
												$base.url + ruleForm.touxiang.split(',')[0]"
											style="width: 120px; height: 120px; border-radius: 50%; object-fit: cover; margin-right: 20px;">
										</el-image>
									</div>
									<div v-else>无头像</div>
								</div>
							</template>
				</el-form-item>
					</el-col>
				</el-row>
			<el-form-item class="btn">
					<el-button v-if="type!='info'" type="primary" @click="onSubmit" style="border: none; cursor: pointer; padding: 0 20px; margin: 0 10px 0 0; color: #fff; border-radius: 4px; background: #409EFF; font-size: 14px; height: 40px;">
					确定
				</el-button>
					<el-button v-if="type!='info'" @click="back()" style="border: 1px solid #dcdfe6; cursor: pointer; padding: 0 20px; margin: 0 10px 0 0; color: #333; border-radius: 4px; background: #fff; font-size: 14px; height: 40px;">
					取消
				</el-button>
					<el-button v-if="type=='info'" @click="back()" style="border: none; cursor: pointer; padding: 0 20px; margin: 0 10px 0 0; color: #fff; border-radius: 4px; background: #909399; font-size: 14px; height: 40px;">
					返回
				</el-button>
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
</style>
