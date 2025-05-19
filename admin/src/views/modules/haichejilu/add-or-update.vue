<template>
	<div class="addEdit-block">
		<template v-if="type=='info'">
			<div class="info-view">
				<div class="info-row"><span class="info-label">订单编号：</span><span class="info-value">{{ruleForm.dingdanbianhao}}</span></div>
				<div class="info-row"><span class="info-label">汽车车牌：</span><span class="info-value">{{ruleForm.qichechepai}}</span></div>
				<div class="info-row"><span class="info-label">汽车类型：</span><span class="info-value">{{ruleForm.qicheleixing}}</span></div>
				<div class="info-row"><span class="info-label">品牌：</span><span class="info-value">{{ruleForm.pinpai}}</span></div>
				<div class="info-row"><span class="info-label">型号：</span><span class="info-value">{{ruleForm.xinghao}}</span></div>
				<div class="info-row"><span class="info-label">日租金：</span><span class="info-value">{{ruleForm.rizujin}}</span></div>
				<div class="info-row"><span class="info-label">租车时间：</span><span class="info-value">{{ruleForm.zucheshijian}}</span></div>
				<div class="info-row"><span class="info-label">还车时间：</span><span class="info-value">{{ruleForm.haicheshijian}}</span></div>
				<div class="info-row"><span class="info-label">还车备注：</span><span class="info-value">{{ruleForm.haichebeizhu}}</span></div>
				<div class="info-row"><span class="info-label">汽车图片：</span>
					<span class="info-value">
						<span v-if="ruleForm.qichetupian">
							<img v-if="ruleForm.qichetupian.substring(0,4)=='http'&&ruleForm.qichetupian.split(',w').length>1" :src="ruleForm.qichetupian" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;box-shadow:0 2px 8px rgba(0,0,0,0.08);">
							<img v-else-if="ruleForm.qichetupian.substring(0,4)=='http'" :src="ruleForm.qichetupian.split(',')[0]" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;box-shadow:0 2px 8px rgba(0,0,0,0.08);">
							<img v-else v-for="(item,index) in ruleForm.qichetupian.split(',')" :key="'qichetupian-img-'+index" :src="$base.url+item" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;box-shadow:0 2px 8px rgba(0,0,0,0.08);">
						</span>
						<span v-else>无图片</span>
					</span>
				</div>
				<div class="info-row"><span class="info-label">车商账号：</span><span class="info-value">{{ruleForm.cheshangzhanghao}}</span></div>
				<div class="info-row"><span class="info-label">用户账号：</span><span class="info-value">{{ruleForm.yonghuzhanghao}}</span></div>
				<div class="info-row"><span class="info-label">用户姓名：</span><span class="info-value">{{ruleForm.yonghuxingming}}</span></div>
				<el-button @click="back()" style="border: none; cursor: pointer; padding: 0 20px; color: #fff; border-radius: 4px; background: #909399; font-size: 14px; height: 40px; margin-top: 24px;">返回</el-button>
			</div>
		</template>
		<template v-else>
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
							<el-form-item class="input" :label="type!='info'?'订单编号':''" prop="dingdanbianhao" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.dingdanbianhao" placeholder="订单编号" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.dingdanbianhao"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">订单编号:</div>
									<div style="padding: 0 12px;">{{ruleForm.dingdanbianhao}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'汽车车牌':''" prop="qichechepai" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.qichechepai" placeholder="汽车车牌" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.qichechepai"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">汽车车牌:</div>
									<div style="padding: 0 12px;">{{ruleForm.qichechepai}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'汽车类型':''" prop="qicheleixing" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.qicheleixing" placeholder="汽车类型" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.qicheleixing"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">汽车类型:</div>
									<div style="padding: 0 12px;">{{ruleForm.qicheleixing}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'品牌':''" prop="pinpai" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.pinpai" placeholder="品牌" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.pinpai"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">品牌:</div>
									<div style="padding: 0 12px;">{{ruleForm.pinpai}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'型号':''" prop="xinghao" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.xinghao" placeholder="型号" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.xinghao"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">型号:</div>
									<div style="padding: 0 12px;">{{ruleForm.xinghao}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'日租金':''" prop="rizujin" :class="type=='info'?'textinfo':''">
								<el-input-number v-if="type!='info'" v-model="ruleForm.rizujin" placeholder="日租金" :precision="2" :step="0.1" :style='{"width":"100%","margin":"0 0 20px"}' :disabled="ro.rizujin"></el-input-number>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">日租金:</div>
									<div style="padding: 0 12px;">￥{{ruleForm.rizujin}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="date" :label="type!='info'?'租车时间':''" prop="zucheshijian" :class="type=='info'?'textinfo':''">
								<el-date-picker v-if="type!='info'"
									value-format="yyyy-MM-dd HH:mm:ss"
									v-model="ruleForm.zucheshijian" 
									type="datetime"
									:readonly="ro.zucheshijian"
									placeholder="租车时间"
									:style='{"width":"100%","margin":"0 0 20px"}'
								></el-date-picker>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">租车时间:</div>
									<div style="padding: 0 12px;">{{ruleForm.zucheshijian}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="date" :label="type!='info'?'还车时间':''" prop="haicheshijian" :class="type=='info'?'textinfo':''">
								<el-date-picker v-if="type!='info'"
									value-format="yyyy-MM-dd HH:mm:ss"
									v-model="ruleForm.haicheshijian" 
									type="datetime"
									:readonly="ro.haicheshijian"
									placeholder="还车时间"
									:style='{"width":"100%","margin":"0 0 20px"}'
								></el-date-picker>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">还车时间:</div>
									<div style="padding: 0 12px;">{{ruleForm.haicheshijian}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="24">
							<el-form-item class="input" :label="type!='info'?'还车备注':''" prop="haichebeizhu" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" type="textarea" :rows="4" v-model="ruleForm.haichebeizhu" placeholder="还车备注" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.haichebeizhu"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">还车备注:</div>
									<div style="padding: 0 12px; white-space: pre-wrap;">{{ruleForm.haichebeizhu}}</div>
								</div>
							</el-form-item>
						</el-col>
						<el-col :span="24">
							<el-form-item class="upload" :label="type!='info'?'汽车图片':''" prop="qichetupian" :class="type=='info'?'textinfo':''">
								<template v-if="type!='info' && !ro.qichetupian">
									<file-upload
										tip="点击上传汽车图片"
										action="file/upload"
										:limit="3"
										:multiple="true"
										:fileUrls="ruleForm.qichetupian?ruleForm.qichetupian:''"
										@change="qichetupianUploadChange"
										:style='{"width":"100%","margin":"0 0 20px"}'
									></file-upload>
								</template>
								<template v-if="type=='info'">
									<div style="font-size: 14px; line-height: 40px; color: #333;">
										<div style="font-weight: bold; margin-bottom: 5px;">汽车图片:</div>
										<div class="images-box" v-if="ruleForm.qichetupian" style="display: flex; flex-wrap: wrap;">
											<el-image 
												v-if="ruleForm.qichetupian.substring(0,4)=='http'&&ruleForm.qichetupian.split(',w').length>1" 
												:src="ruleForm.qichetupian" 
												:preview-src-list="[ruleForm.qichetupian]"
												style="width: 120px; height: 120px; border-radius: 10px; object-fit: cover; margin-right: 20px; margin-bottom: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
											></el-image>
											<el-image 
												v-else-if="ruleForm.qichetupian.substring(0,4)=='http'" 
												:src="ruleForm.qichetupian.split(',')[0]" 
												:preview-src-list="[ruleForm.qichetupian.split(',')[0]]"
												style="width: 120px; height: 120px; border-radius: 10px; object-fit: cover; margin-right: 20px; margin-bottom: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
											></el-image>
											<el-image 
												v-else
												v-for="(item,index) in ruleForm.qichetupian.split(',')" 
												:key="index"
												:src="$base.url+item" 
												:preview-src-list="[$base.url+item]"
												style="width: 120px; height: 120px; border-radius: 10px; object-fit: cover; margin-right: 20px; margin-bottom: 20px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
											></el-image>
										</div>
										<div v-else>无图片</div>
									</div>
								</template>
							</el-form-item>
						</el-col>
						<el-col :span="12">
							<el-form-item class="input" :label="type!='info'?'车商账号':''" prop="cheshangzhanghao" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.cheshangzhanghao" placeholder="车商账号" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.cheshangzhanghao"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">车商账号:</div>
									<div style="padding: 0 12px;">{{ruleForm.cheshangzhanghao}}</div>
								</div>
							</el-form-item>
						</el-col>
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
							<el-form-item class="input" :label="type!='info'?'用户姓名':''" prop="yonghuxingming" :class="type=='info'?'textinfo':''">
								<el-input v-if="type!='info'" v-model="ruleForm.yonghuxingming" placeholder="用户姓名" clearable :style='{"width":"100%","padding":"0 12px","margin":"0 0 20px","borderColor":"#dcdfe6","outline":"none","fontSize":"14px","height":"40px","color":"#333","borderRadius":"4px","borderWidth":"1px"}' :readonly="ro.yonghuxingming"></el-input>
								<div v-else style="font-size: 14px; line-height: 40px; color: #333;">
									<div style="font-weight: bold; margin-bottom: 5px;">用户姓名:</div>
									<div style="padding: 0 12px;">{{ruleForm.yonghuxingming}}</div>
								</div>
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
					</el-form-item>
				</el-form>
			</el-card>
		</template>
	</div>
</template>

<script>
	import { 
		isNumber,
	} from "@/utils/validate";
	export default {
		data() {
			var validateNumber = (rule, value, callback) => {
				if(!value){
					callback();
				} else if (!isNumber(value)) {
					callback(new Error("请输入数字"));
				} else {
					callback();
				}
			};
			return {
				id: '',
				type: '',
				ro:{
					dingdanbianhao : false,
					qichechepai : false,
					qicheleixing : false,
					pinpai : false,
					xinghao : false,
					rizujin : false,
					zucheshijian : false,
					haicheshijian : false,
					haichebeizhu : false,
					qichetupian : false,
					cheshangzhanghao : false,
					yonghuzhanghao : false,
					yonghuxingming : false,
					crossuserid : false,
					crossrefid : false,
					sfsh : false,
					shhf : false,
				},
				ruleForm: {
					dingdanbianhao: '',
					qichechepai: '',
					qicheleixing: '',
					pinpai: '',
					xinghao: '',
					rizujin: '',
					zucheshijian: '',
					haicheshijian: '',
					haichebeizhu: '',
					qichetupian: '',
					cheshangzhanghao: '',
					yonghuzhanghao: '',
					yonghuxingming: '',
					crossuserid: '',
					crossrefid: '',
					shhf: '',
				},
				rules: {
					dingdanbianhao: [
					],
					qichechepai: [
					],
					qicheleixing: [
					],
					pinpai: [
					],
					xinghao: [
					],
					rizujin: [
						{ validator: validateNumber, trigger: 'blur' },
					],
					zucheshijian: [
					],
					haicheshijian: [
					],
					haichebeizhu: [
					],
					qichetupian: [
					],
					cheshangzhanghao: [
					],
					yonghuzhanghao: [
					],
					yonghuxingming: [
					],
					crossuserid: [
					],
					crossrefid: [
					],
					shhf: [
					],
				},
			};
		},
		props: ["parent"],
		computed: {
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
				if(this.type=='info'||this.type=='else'){
					this.info(id);
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
			},
			// 多级联动参数
			info(id) {
				let _this =this;
				_this.$http({
					url: `haichejilu/info/${id}`,
					method: "get"
				}).then(({ data }) => {
					if (data && data.code === 0) {
						_this.ruleForm = data.data;
					} else {
						_this.$message.error(data.msg);
					}
				});
			},
			// 提交
			onSubmit() {
				this.$refs["ruleForm"].validate(valid => {
					if (valid) {
						this.$http({
							url: `haichejilu/${!this.ruleForm.id ? "save" : "update"}`,
							method: "post",
							data: this.ruleForm
						}).then(({ data }) => {
							if (data && data.code === 0) {
								this.$message({
									message: "操作成功",
									type: "success",
									duration: 1500,
									onClose: () => {
										this.parent.showFlag = true;
										this.parent.addOrUpdateFlag = false;
										this.parent.haichejiluCrossAddOrUpdateFlag = false;
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
				this.parent.haichejiluCrossAddOrUpdateFlag = false;
				this.parent.contentStyleChange();
			},
			qichetupianUploadChange(fileUrls){
				this.ruleForm.qichetupian = fileUrls;
			},
		}
	};
</script>
<style lang="css" scoped>
	.el-date-editor.el-input {
		width: 100%;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
		padding: 0 12px 0 0;
		color: #606266;
		font-weight: 500;
		width: 120px;
		font-size: 14px;
		line-height: 40px;
		text-align: right;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
		margin-left: 120px;
	}
	.info-view {
		margin-bottom: 10px;
	}
	.info-row {
		display: flex;
		align-items: center;
		margin-bottom: 18px;
	}
	.info-label {
		width: 180px;
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
</style>
