<template>
	<div class="dealer-container">
		<!-- 信息查看模式 -->
		<template v-if="type=='info'">
			<el-card class="dealer-card" shadow="hover">
				<div slot="header" class="card-header">
					<i class="el-icon-info primary-icon"></i>
					<span>车商详细信息</span>
				</div>
				<div class="card-content">
					<div class="dealer-profile">
						<div class="dealer-avatar">
							<img v-if="ruleForm.touxiang" :src="getAvatarUrl(ruleForm.touxiang)" class="avatar-image">
							<div v-else class="avatar-placeholder">
								<i class="el-icon-user-solid"></i>
				</div>
			</div>
						<div class="dealer-info">
							<h2 class="dealer-name">{{ruleForm.cheshangxingming}}</h2>
							<div class="dealer-id">
								<i class="el-icon-user"></i>
								<span>{{ruleForm.cheshangzhanghao}}</span>
							</div>
							<div class="dealer-meta">
								<div class="meta-item">
									<i class="el-icon-male"></i>
									<span>{{ruleForm.xingbie}}</span>
								</div>
								<div class="meta-item">
									<i class="el-icon-mobile-phone"></i>
									<span>{{ruleForm.shouji}}</span>
								</div>
							</div>
						</div>
					</div>
					
					<div class="dealer-details">
						<div class="detail-section">
							<h3 class="section-title">身份信息</h3>
							<div class="detail-item">
								<span class="detail-label">身份证</span>
								<span class="detail-value">{{ruleForm.shenfenzheng}}</span>
							</div>
						</div>
						
						<div class="detail-section">
							<h3 class="section-title">资质文件</h3>
							<div class="detail-item">
								<el-button v-if="ruleForm.zizhiwenjian" type="primary" size="small" plain icon="el-icon-document" @click="download($base.url+ruleForm.zizhiwenjian)">
									查看文件
								</el-button>
								<span v-else class="no-file">暂无资质文件</span>
							</div>
						</div>
					</div>
				</div>
				<div class="card-actions">
					<el-button type="primary" icon="el-icon-back" @click="back()">返回列表</el-button>
				</div>
			</el-card>
		</template>
		
		<!-- 编辑/添加模式 -->
		<template v-else>
			<el-card class="dealer-card" shadow="hover">
				<div slot="header" class="card-header">
					<i class="el-icon-edit success-icon"></i>
					<span>{{id ? '编辑' : '添加'}}车商信息</span>
				</div>
				<el-form 
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
					label-position="top"
					class="dealer-form"
					size="medium"
				>
					<!-- 账号信息 -->
					<div class="form-section">
						<h3 class="section-title">
							<i class="el-icon-user"></i>
							账号信息
						</h3>
						<div class="form-grid">
							<el-form-item label="车商账号" prop="cheshangzhanghao">
								<el-input v-model="ruleForm.cheshangzhanghao" placeholder="请输入车商账号"></el-input>
				</el-form-item>
							<el-form-item label="密码" prop="mima">
								<el-input v-model="ruleForm.mima" placeholder="请输入密码" type="password" show-password></el-input>
								<div v-if="id" class="form-tip">留空则不修改密码</div>
				</el-form-item>
						</div>
					</div>
					
					<!-- 基本信息 -->
					<div class="form-section">
						<h3 class="section-title">
							<i class="el-icon-notebook-1"></i>
							基本信息
						</h3>
						<div class="form-grid">
							<el-form-item label="车商姓名" prop="cheshangxingming">
								<el-input v-model="ruleForm.cheshangxingming" placeholder="请输入车商姓名"></el-input>
				</el-form-item>
							<el-form-item label="性别" prop="xingbie">
								<el-select v-model="ruleForm.xingbie" placeholder="请选择性别" class="full-width">
						<el-option
							v-for="(item,index) in xingbieOptions"
							v-bind:key="index"
							:label="item"
							:value="item">
						</el-option>
					</el-select>
				</el-form-item>
						</div>
						<div class="form-grid">
							<el-form-item label="手机" prop="shouji">
								<el-input v-model="ruleForm.shouji" placeholder="请输入手机号码">
									<template slot="prefix">
										<i class="el-icon-mobile-phone prefix-icon"></i>
									</template>
								</el-input>
				</el-form-item>
							<el-form-item label="身份证" prop="shenfenzheng">
								<el-input v-model="ruleForm.shenfenzheng" placeholder="请输入身份证号码">
									<template slot="prefix">
										<i class="el-icon-postcard prefix-icon"></i>
									</template>
								</el-input>
				</el-form-item>
						</div>
					</div>
					
					<!-- 媒体文件 -->
					<div class="form-section">
						<h3 class="section-title">
							<i class="el-icon-picture-outline"></i>
							媒体文件
						</h3>
						<div class="form-grid">
							<el-form-item label="头像" prop="touxiang">
								<div class="media-uploader">
									<div v-if="ruleForm.touxiang" class="avatar-preview">
										<img :src="getAvatarUrl(ruleForm.touxiang)" class="avatar-image">
										<div class="avatar-overlay">
											<el-button type="danger" circle icon="el-icon-delete" size="mini" @click="clearAvatar"></el-button>
										</div>
									</div>
									<el-upload
										v-else
										class="avatar-upload"
						action="file/upload"
										:show-file-list="false"
										:on-success="handleAvatarSuccess"
									>
										<div class="upload-placeholder">
											<i class="el-icon-plus"></i>
											<p>上传头像</p>
										</div>
									</el-upload>
								</div>
				</el-form-item>  
							
							<el-form-item label="资质文件" prop="zizhiwenjian">
								<div class="media-uploader">
									<div v-if="ruleForm.zizhiwenjian" class="file-preview">
										<div class="file-icon">
											<i class="el-icon-document"></i>
										</div>
										<div class="file-info">
											<div class="file-name">已上传资质文件</div>
											<div class="file-actions">
												<el-button type="primary" size="mini" plain icon="el-icon-view" @click="download($base.url+ruleForm.zizhiwenjian)">查看</el-button>
												<el-button type="danger" size="mini" plain icon="el-icon-delete" @click="clearFile">删除</el-button>
											</div>
										</div>
									</div>
									<el-upload
										v-else
										class="file-upload"
						action="file/upload"
										:show-file-list="false"
										:on-success="handleFileSuccess"
									>
										<div class="upload-placeholder">
											<i class="el-icon-upload"></i>
											<p>上传资质文件</p>
										</div>
									</el-upload>
									<div v-if="id" class="form-tip">编辑模式下资质文件可选，留空则不更新</div>
								</div>
				</el-form-item>
						</div>
					</div>
					
					<!-- 按钮区域 -->
					<div class="form-actions">
						<el-button @click="back()" plain icon="el-icon-close">取 消</el-button>
						<el-button type="primary" @click="onSubmit" icon="el-icon-check">确 定</el-button>
					</div>
		</el-form>
			</el-card>
		</template>
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
				formBackup: null, // 用于保存表单原始数据			
				ro:{
					cheshangzhanghao : false,
					mima : false,
					cheshangxingming : false,
					xingbie : false,
					shouji : false,
					shenfenzheng : false,
					zizhiwenjian : false,
					touxiang : false,
					sfsh : false,
					shhf : false,
				},
			
				ruleForm: {
					cheshangzhanghao: '',
					mima: '',
					cheshangxingming: '',
					xingbie: '',
					shouji: '',
					shenfenzheng: '',
					zizhiwenjian: '',
					touxiang: '',
					shhf: '',
				},
				xingbieOptions: [],

				rules: {
					cheshangzhanghao: [
						{ required: true, message: '车商账号不能为空', trigger: 'blur' },
					],
					mima: [
						{ required: function() { return !this.id; }, message: '密码不能为空', trigger: 'blur' },
					],
					cheshangxingming: [
						{ required: true, message: '车商姓名不能为空', trigger: 'blur' },
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
					zizhiwenjian: [
						// 自定义验证器
						{ validator: (rule, value, callback) => {
							// 如果是新增模式下，且资质文件为空，则报错
							if (!this.id && (!value || value === '')) {
								callback(new Error('资质文件不能为空'));
							} else {
								// 编辑模式下或资质文件不为空
								callback();
							}
						}, trigger: 'blur' }
					],
					touxiang: [],
					sfsh: [],
					shhf: [],
				},
			};
		},
		props: ["parent"],
		computed: {},
		components: {},
		created() {
		},
		methods: {
			// 下载文件
			download(file){
				window.open(`${file}`)
			},
			// 获取头像URL
			getAvatarUrl(touxiang) {
				if (!touxiang) {
					return '';
				}
				
				// 多图只取第一个
				let url = touxiang.split(',')[0];
				
				// 如果URL包含http，直接返回
				if (url.startsWith('http')) {
					return url;
				}
				
				// 否则拼接baseUrl
				return this.$base.url + url;
			},
			// 清除头像
			clearAvatar() {
				if (this.id) {
					this.ruleForm.touxiang = '';
					this.$nextTick(() => {
						this.$refs.ruleForm.validateField('touxiang');
					});
				}
			},
			// 清除文件
			clearFile() {
				if (this.id) {
					this.ruleForm.zizhiwenjian = '';
					// 手动触发校验 
					this.$nextTick(() => {
						this.$refs.ruleForm.validateField('zizhiwenjian');
					});
				}
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
						if(o=='cheshangzhanghao'){
							this.ruleForm.cheshangzhanghao = obj[o];
							this.ro.cheshangzhanghao = true;
							continue;
						}
						if(o=='mima'){
							this.ruleForm.mima = obj[o];
							this.ro.mima = true;
							continue;
						}
						if(o=='cheshangxingming'){
							this.ruleForm.cheshangxingming = obj[o];
							this.ro.cheshangxingming = true;
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
						if(o=='zizhiwenjian'){
							this.ruleForm.zizhiwenjian = obj[o];
							this.ro.zizhiwenjian = true;
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
					url: `cheshang/info/${id}`,
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

			// 处理头像上传成功
			handleAvatarSuccess(res, file) {
				if(res.code == 0){
					this.ruleForm.touxiang = res.data;
				} else {
					this.$message.error(res.msg);
					}
			},
			
			// 处理文件上传成功
			handleFileSuccess(res, file) {
				if(res.code == 0){
					this.ruleForm.zizhiwenjian = res.data;
				} else {
					this.$message.error(res.msg);
				}
			},

			// 提交
			async onSubmit() {
					await this.$refs["ruleForm"].validate(async valid => {
						if (valid) {
						let submitData = JSON.parse(JSON.stringify(this.ruleForm));
						
						// 处理文件路径
						if(submitData.zizhiwenjian!=null && submitData.zizhiwenjian !== '') {
							submitData.zizhiwenjian = submitData.zizhiwenjian.replace(new RegExp(this.$base.url,"g"),"");
						}
						if(submitData.touxiang!=null && submitData.touxiang !== '') {
							submitData.touxiang = submitData.touxiang.replace(new RegExp(this.$base.url,"g"),"");
						}
						
						// 处理更新时的数据
						if (submitData.id) {
							// 如果密码为空，不更新密码字段
							if (!submitData.mima) {
								delete submitData.mima;
											}
										}
						
						// 总是记录原始表单数据，防止有些情况未处理
						this.formBackup = JSON.parse(JSON.stringify(submitData));
							
							await this.$http({
							url: `cheshang/${!submitData.id ? "save" : "update"}`,
								method: "post",
							data: submitData
							}).then(async ({ data }) => {
								if (data && data.code === 0) {
									this.$message({
										message: "操作成功",
										type: "success",
										duration: 1500,
										onClose: () => {
										this.back();
										}
									});
								} else {
									this.$message.error(data.msg);
								}
							});
					} else {
						return false;
						}
					});
			},
			
			// 返回
			back() {
				this.parent.showFlag = true;
				this.parent.addOrUpdateFlag = false;
				this.parent.cheshangCrossAddOrUpdateFlag = false;
				this.parent.contentStyleChange();
			}
		}
	};
</script>

<style lang="css" scoped>
/* 主色彩变量 */
/* 容器样式 */
.dealer-container {
    padding: 24px;
    max-width: 1200px;
    margin: 0 auto;
}

/* 卡片样式 */
.dealer-card {
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.05);
    transition: transform 0.3s ease, box-shadow 0.3s ease;
	}
	
.dealer-card:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.08);
}

.card-header {
    display: flex;
    align-items: center;
    font-size: 18px;
    font-weight: 600;
    color: #303133;
    border-bottom: 1px solid #ebeef5;
    padding: 16px 20px;
    background-color: rgba(64, 158, 255, 0.02);
}

.card-header i {
    margin-right: 10px;
    font-size: 20px;
}

.primary-icon {
    color: #409EFF;
	}
	
.success-icon {
    color: #67C23A;
}

.card-content {
    padding: 24px;
	}

.card-actions {
    padding: 16px 24px;
    border-top: 1px solid #ebeef5;
    text-align: right;
    background-color: #fafafa;
	}
	
/* 车商信息查看模式 */
.dealer-profile {
    display: flex;
    align-items: flex-start;
    margin-bottom: 30px;
    padding-bottom: 20px;
    border-bottom: 1px dashed #ebeef5;
}

.dealer-avatar {
    width: 100px;
    height: 100px;
    margin-right: 24px;
    border-radius: 12px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(64, 158, 255, 0.15);
    flex-shrink: 0;
    transition: transform 0.3s ease;
	}

.dealer-avatar:hover {
    transform: scale(1.05);
}

.avatar-image {
		width: 100%;
    height: 100%;
    object-fit: cover;
}

.avatar-placeholder {
		width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #f0f2f5;
    color: #909399;
    font-size: 36px;
}

.dealer-info {
    flex: 1;
}

.dealer-name {
    font-size: 22px;
    margin: 0 0 12px;
    font-weight: 600;
    color: #303133;
	}

.dealer-id {
    display: inline-flex;
    align-items: center;
    padding: 4px 12px;
    background: rgba(64, 158, 255, 0.1);
    border-radius: 20px;
    color: #409EFF;
    font-size: 14px;
    margin-bottom: 16px;
	}

.dealer-id i {
    margin-right: 6px;
}

.dealer-meta {
    display: flex;
    flex-wrap: wrap;
    gap: 16px;
}

.meta-item {
    display: flex;
    align-items: center;
    color: #606266;
		font-size: 15px;
}

.meta-item i {
    margin-right: 6px;
    color: #909399;
	}

.detail-section {
    margin-bottom: 24px;
	}

.section-title {
			font-size: 16px;
    font-weight: 600;
    color: #303133;
    margin: 0 0 16px;
    padding-bottom: 8px;
    border-bottom: 2px solid rgba(64, 158, 255, 0.1);
}

.detail-item {
    display: flex;
    align-items: center;
    margin-bottom: 12px;
    padding: 10px 16px;
    background-color: #f8fafc;
    border-radius: 8px;
}

.detail-label {
    width: 80px;
    color: #606266;
    font-weight: 500;
    flex-shrink: 0;
		}

.detail-value {
    flex: 1;
    color: #303133;
    word-break: break-all;
	}

.no-file {
    color: #909399;
    font-style: italic;
}

/* 表单样式 */
.dealer-form .form-section {
    margin-bottom: 32px;
}

.dealer-form .section-title {
    font-size: 16px;
    font-weight: 600;
    color: #303133;
    margin: 0 0 20px;
    display: flex;
    align-items: center;
    position: relative;
	}

.dealer-form .section-title i {
    margin-right: 8px;
    color: #409EFF;
}

.dealer-form .section-title::after {
    content: '';
    flex: 1;
    height: 1px;
    background: linear-gradient(to right, rgba(64, 158, 255, 0.1), rgba(64, 158, 255, 0));
    margin-left: 12px;
	}
	
.form-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 20px;
    margin-bottom: 20px;
}

@media (max-width: 768px) {
    .form-grid {
        grid-template-columns: 1fr;
    }
}

.form-tip {
    font-size: 13px;
    color: #909399;
    margin-top: 4px;
    line-height: 1.3;
}

.full-width {
    width: 100%;
	}

.prefix-icon {
    color: #909399;
	}
	
/* 媒体上传部分 */
.media-uploader {
    border-radius: 8px;
    overflow: hidden;
}

/* 头像预览与上传 */
.avatar-preview {
    position: relative;
    width: 120px;
		height: 120px;
    border-radius: 8px;
    overflow: hidden;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.avatar-preview .avatar-image {
    width: 100%;
    height: 100%;
    object-fit: cover;
    transition: transform 0.3s ease;
			}

.avatar-preview .avatar-overlay {
    position: absolute;
    bottom: 0;
    left: 0;
    right: 0;
    background: rgba(0, 0, 0, 0.5);
    padding: 6px;
    display: flex;
    justify-content: center;
    transform: translateY(100%);
    transition: transform 0.3s ease;
}

.avatar-preview:hover .avatar-image {
    transform: scale(1.1);
			}

.avatar-preview:hover .avatar-overlay {
    transform: translateY(0);
		}

.avatar-upload, .file-upload {
    width: 100%;
			cursor: pointer;
}

/* 文件预览 */
.file-preview {
    display: flex;
    align-items: center;
    padding: 16px;
    background-color: #f8f9fa;
    border-radius: 8px;
    border: 1px dashed #ebeef5;
}

.file-icon {
    width: 50px;
    height: 50px;
    background-color: rgba(64, 158, 255, 0.1);
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 16px;
    flex-shrink: 0;
		}

.file-icon i {
    font-size: 24px;
    color: #409EFF;
}

.file-info {
    flex: 1;
}

.file-name {
    font-weight: 500;
    color: #606266;
    margin-bottom: 10px;
}

.file-actions {
    display: flex;
    gap: 10px;
}

/* 上传占位符 */
.upload-placeholder {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 30px;
    background-color: #f8f9fa;
    border: 2px dashed #ebeef5;
    border-radius: 8px;
    transition: all 0.3s ease;
			cursor: pointer;
}

.upload-placeholder i {
    font-size: 28px;
    color: #909399;
    margin-bottom: 8px;
    transition: all 0.3s ease;
		}

.upload-placeholder p {
    color: #909399;
    margin: 0;
				font-size: 14px;
}

.upload-placeholder:hover {
    background-color: rgba(64, 158, 255, 0.05);
    border-color: #409EFF;
		}

.upload-placeholder:hover i {
    color: #409EFF;
    transform: scale(1.1);
	}

/* 表单操作按钮 */
.form-actions {
		display: flex;
    justify-content: center;
    gap: 16px;
    padding-top: 30px;
    margin-top: 10px;
    border-top: 1px dashed #ebeef5;
	}

.form-actions .el-button {
    padding: 12px 24px;
    min-width: 120px;
    transition: all 0.3s ease;
	}

.form-actions .el-button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
	}
</style>
