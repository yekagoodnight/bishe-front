<template>
	<div class="addEdit-block">
		<template v-if="type=='info'">
			<div class="info-view">
				<div class="info-row"><span class="info-label">订单编号：</span><span class="info-value">{{ruleForm.dingdanbianhao}}</span></div>
				<div class="info-row"><span class="info-label">汽车车牌：</span><span class="info-value">{{ruleForm.qichechepai}}</span></div>
				<div class="info-row"><span class="info-label">汽车类型：</span><span class="info-value">{{ruleForm.qicheleixing}}</span></div>
				<div class="info-row"><span class="info-label">品牌：</span><span class="info-value">{{ruleForm.pinpai}}</span></div>
				<div class="info-row"><span class="info-label">型号：</span><span class="info-value">{{ruleForm.xinghao}}</span></div>
				<div class="info-row"><span class="info-label">年份：</span><span class="info-value">{{ruleForm.nianfen}}</span></div>
				<div class="info-row"><span class="info-label">日租金：</span><span class="info-value">{{ruleForm.rizujin}}</span></div>
				<div class="info-row"><span class="info-label">天数：</span><span class="info-value">{{ruleForm.tianshu}}</span></div>
				<div class="info-row"><span class="info-label">应付价格：</span><span class="info-value">{{ruleForm.yingfujiage}}</span></div>
				<div class="info-row"><span class="info-label">汽车图片：</span>
					<span class="info-value">
						<span v-if="ruleForm.qichetupian">
							<img v-if="ruleForm.qichetupian.substring(0,4)=='http'&&ruleForm.qichetupian.split(',w').length>1" :src="ruleForm.qichetupian" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;" key="qichetupian-img1">
							<img v-else-if="ruleForm.qichetupian.substring(0,4)=='http'" :src="ruleForm.qichetupian.split(',')[0]" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;" key="qichetupian-img2">
							<img v-else v-for="(item,index) in ruleForm.qichetupian.split(',')" :key="'qichetupian-img-'+index" :src="$base.url+item" width="60" height="60" style="border-radius:8px;object-fit:cover;margin-right:8px;">
						</span>
						<span v-else>无图片</span>
					</span>
				</div>
				<div class="info-row"><span class="info-label">租车时间：</span><span class="info-value">{{ruleForm.zucheshijian}}</span></div>
				<div class="info-row"><span class="info-label">租车备注：</span><span class="info-value">{{ruleForm.zuchebeizhu}}</span></div>
				<div class="info-row"><span class="info-label">车商账号：</span><span class="info-value">{{ruleForm.cheshangzhanghao}}</span></div>
				<div class="info-row"><span class="info-label">用户账号：</span><span class="info-value">{{ruleForm.yonghuzhanghao}}</span></div>
				<div class="info-row"><span class="info-label">用户姓名：</span><span class="info-value">{{ruleForm.yonghuxingming}}</span></div>
				<el-button @click="back()" style="border: none; cursor: pointer; padding: 0 20px; color: #fff; border-radius: 4px; background: #909399; font-size: 14px; height: 40px; margin-top: 24px;">返回</el-button>
			</div>
		</template>
		<el-form v-else
			class="add-update-preview"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
			label-width="180px"
			label-position="right"
			style="max-width: 800px; margin: 0 auto;"
		>
			<template >
				<el-form-item class="input" v-if="type!='info'" label="订单编号" prop="dingdanbianhao" >
					<el-input v-model="ruleForm.dingdanbianhao" placeholder="订单编号" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-else-if="ruleForm.dingdanbianhao" label="订单编号" prop="dingdanbianhao" >
					<el-input v-model="ruleForm.dingdanbianhao" placeholder="订单编号" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="汽车车牌" prop="qichechepai" >
					<el-input v-model="ruleForm.qichechepai" placeholder="汽车车牌" clearable  :readonly="ro.qichechepai"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="汽车车牌" prop="qichechepai" >
					<el-input v-model="ruleForm.qichechepai" placeholder="汽车车牌" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="汽车类型" prop="qicheleixing" >
					<el-input v-model="ruleForm.qicheleixing" placeholder="汽车类型" clearable  :readonly="ro.qicheleixing"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="汽车类型" prop="qicheleixing" >
					<el-input v-model="ruleForm.qicheleixing" placeholder="汽车类型" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="品牌" prop="pinpai" >
					<el-input v-model="ruleForm.pinpai" placeholder="品牌" clearable  :readonly="ro.pinpai"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="品牌" prop="pinpai" >
					<el-input v-model="ruleForm.pinpai" placeholder="品牌" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="型号" prop="xinghao" >
					<el-input v-model="ruleForm.xinghao" placeholder="型号" clearable  :readonly="ro.xinghao"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="型号" prop="xinghao" >
					<el-input v-model="ruleForm.xinghao" placeholder="型号" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="年份" prop="nianfen" >
					<el-input v-model="ruleForm.nianfen" placeholder="年份" clearable  :readonly="ro.nianfen"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="年份" prop="nianfen" >
					<el-input v-model="ruleForm.nianfen" placeholder="年份" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="日租金" prop="rizujin" >
					<el-input-number v-model="ruleForm.rizujin" placeholder="日租金" :disabled="ro.rizujin"></el-input-number>
				</el-form-item>
				<el-form-item v-else class="input" label="日租金" prop="rizujin" >
					<el-input v-model="ruleForm.rizujin" placeholder="日租金" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="天数" prop="tianshu" >
					<el-input v-model.number="ruleForm.tianshu" placeholder="天数" clearable  :readonly="ro.tianshu"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="天数" prop="tianshu" >
					<el-input v-model="ruleForm.tianshu" placeholder="天数" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="应付价格" prop="yingfujiage" >
					<el-input v-model="yingfujiage" placeholder="应付价格" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-else-if="ruleForm.yingfujiage" label="应付价格" prop="yingfujiage" >
					<el-input v-model="ruleForm.yingfujiage" placeholder="应付价格" readonly></el-input>
				</el-form-item>
				<el-form-item class="upload" v-if="type!='info' && !ro.qichetupian" label="汽车图片" prop="qichetupian" >
					<file-upload
						tip="点击上传汽车图片"
						action="file/upload"
						:limit="3"
						:multiple="true"
						:fileUrls="ruleForm.qichetupian?ruleForm.qichetupian:''"
						@change="qichetupianUploadChange"
					></file-upload>
				</el-form-item>
				<el-form-item class="upload" v-else-if="ruleForm.qichetupian" label="汽车图片" prop="qichetupian" >
					<img v-if="ruleForm.qichetupian.substring(0,4)=='http'&&ruleForm.qichetupian.split(',w').length>1" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.qichetupian" width="100" height="100">
					<img v-else-if="ruleForm.qichetupian.substring(0,4)=='http'" class="upload-img" style="margin-right:20px;" v-bind:key="index" :src="ruleForm.qichetupian.split(',')[0]" width="100" height="100">
					<img v-else class="upload-img" style="margin-right:20px;" v-bind:key="index" v-for="(item,index) in ruleForm.qichetupian.split(',')" :src="$base.url+item" width="100" height="100">
				</el-form-item>
				<el-form-item class="date" v-if="type!='info'" label="租车时间" prop="zucheshijian" >
					<el-date-picker
						value-format="yyyy-MM-dd HH:mm:ss"
						v-model="ruleForm.zucheshijian" 
						type="datetime"
						:readonly="ro.zucheshijian"
						placeholder="租车时间"
					></el-date-picker>
				</el-form-item>
				<el-form-item class="input" v-else-if="ruleForm.zucheshijian" label="租车时间" prop="zucheshijian" >
					<el-input v-model="ruleForm.zucheshijian" placeholder="租车时间" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="租车备注" prop="zuchebeizhu" >
					<el-input v-model="ruleForm.zuchebeizhu" placeholder="租车备注" clearable  :readonly="ro.zuchebeizhu"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="租车备注" prop="zuchebeizhu" >
					<el-input v-model="ruleForm.zuchebeizhu" placeholder="租车备注" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="车商账号" prop="cheshangzhanghao" >
					<el-input v-model="ruleForm.cheshangzhanghao" placeholder="车商账号" clearable  :readonly="ro.cheshangzhanghao"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="车商账号" prop="cheshangzhanghao" >
					<el-input v-model="ruleForm.cheshangzhanghao" placeholder="车商账号" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="用户账号" prop="yonghuzhanghao" >
					<el-input v-model="ruleForm.yonghuzhanghao" placeholder="用户账号" clearable  :readonly="ro.yonghuzhanghao"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="用户账号" prop="yonghuzhanghao" >
					<el-input v-model="ruleForm.yonghuzhanghao" placeholder="用户账号" readonly></el-input>
				</el-form-item>
				<el-form-item class="input" v-if="type!='info'"  label="用户姓名" prop="yonghuxingming" >
					<el-input v-model="ruleForm.yonghuxingming" placeholder="用户姓名" clearable  :readonly="ro.yonghuxingming"></el-input>
				</el-form-item>
				<el-form-item v-else class="input" label="用户姓名" prop="yonghuxingming" >
					<el-input v-model="ruleForm.yonghuxingming" placeholder="用户姓名" readonly></el-input>
				</el-form-item>
			</template>
			<el-form-item class="btn">
				<el-button class="btn3"  v-if="type!='info'" type="success" @click="onSubmit">
					<span class="icon iconfont icon-xihuan"></span>
					确定
				</el-button>
				<el-button class="btn4" v-if="type!='info'" type="success" @click="back()">
					<span class="icon iconfont icon-xihuan"></span>
					取消
				</el-button>
				<el-button class="btn5" v-if="type=='info'" type="success" @click="back()">
					<span class="icon iconfont icon-xihuan"></span>
					返回
				</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>
<script>
	import { 
		isNumber,
		isIntNumer,
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
			var validateIntNumber = (rule, value, callback) => {
				if(!value){
					callback();
				} else if (!isIntNumer(value)) {
					callback(new Error("请输入整数"));
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
					nianfen : false,
					rizujin : false,
					tianshu : false,
					yingfujiage : false,
					qichetupian : false,
					zucheshijian : false,
					zuchebeizhu : false,
					cheshangzhanghao : false,
					yonghuzhanghao : false,
					yonghuxingming : false,
					ispay : false,
					userid : false,
				},
			
				ruleForm: {
					dingdanbianhao: this.getUUID(),
					qichechepai: '',
					qicheleixing: '',
					pinpai: '',
					xinghao: '',
					nianfen: '',
					rizujin: '',
					tianshu: '',
					yingfujiage: '',
					qichetupian: '',
					zucheshijian: '',
					zuchebeizhu: '',
					cheshangzhanghao: '',
					yonghuzhanghao: '',
					yonghuxingming: '',
					userid: '',
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
					nianfen: [
					],
					rizujin: [
						{ validator: validateNumber, trigger: 'blur' },
					],
					tianshu: [
						{ required: true, message: '天数不能为空', trigger: 'blur' },
						{ validator: validateIntNumber, trigger: 'blur' },
					],
					yingfujiage: [
						{ validator: validateNumber, trigger: 'blur' },
					],
					qichetupian: [
					],
					zucheshijian: [
					],
					zuchebeizhu: [
						{ required: true, message: '租车备注不能为空', trigger: 'blur' },
					],
					cheshangzhanghao: [
					],
					yonghuzhanghao: [
					],
					yonghuxingming: [
					],
					ispay: [
					],
					userid: [
					],
				},
			};
		},
		props: ["parent"],
		computed: {


			yingfujiage:{
				get: function () {
					return 1*this.ruleForm.rizujin*this.ruleForm.tianshu
				}
			},

		},
		components: {
		},
		created() {
			this.ruleForm.zucheshijian = this.getCurDateTime()
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
						if(o=='dingdanbianhao'){
							this.ruleForm.dingdanbianhao = obj[o];
							this.ro.dingdanbianhao = true;
							continue;
						}
						if(o=='qichechepai'){
							this.ruleForm.qichechepai = obj[o];
							this.ro.qichechepai = true;
							continue;
						}
						if(o=='qicheleixing'){
							this.ruleForm.qicheleixing = obj[o];
							this.ro.qicheleixing = true;
							continue;
						}
						if(o=='pinpai'){
							this.ruleForm.pinpai = obj[o];
							this.ro.pinpai = true;
							continue;
						}
						if(o=='xinghao'){
							this.ruleForm.xinghao = obj[o];
							this.ro.xinghao = true;
							continue;
						}
						if(o=='nianfen'){
							this.ruleForm.nianfen = obj[o];
							this.ro.nianfen = true;
							continue;
						}
						if(o=='rizujin'){
							this.ruleForm.rizujin = obj[o];
							this.ro.rizujin = true;
							continue;
						}
						if(o=='tianshu'){
							this.ruleForm.tianshu = obj[o];
							this.ro.tianshu = true;
							continue;
						}
						if(o=='yingfujiage'){
							this.ruleForm.yingfujiage = obj[o];
							this.ro.yingfujiage = true;
							continue;
						}
						if(o=='qichetupian'){
							this.ruleForm.qichetupian = obj[o];
							this.ro.qichetupian = true;
							continue;
						}
						if(o=='zucheshijian'){
							this.ruleForm.zucheshijian = obj[o];
							this.ro.zucheshijian = true;
							continue;
						}
						if(o=='zuchebeizhu'){
							this.ruleForm.zuchebeizhu = obj[o];
							this.ro.zuchebeizhu = true;
							continue;
						}
						if(o=='cheshangzhanghao'){
							this.ruleForm.cheshangzhanghao = obj[o];
							this.ro.cheshangzhanghao = true;
							continue;
						}
						if(o=='yonghuzhanghao'){
							this.ruleForm.yonghuzhanghao = obj[o];
							this.ro.yonghuzhanghao = true;
							continue;
						}
						if(o=='yonghuxingming'){
							this.ruleForm.yonghuxingming = obj[o];
							this.ro.yonghuxingming = true;
							continue;
						}
						if(o=='userid'){
							this.ruleForm.userid = obj[o];
							this.ro.userid = true;
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
						if(this.$storage.get("role")!="管理员") {
							this.ro.dingdanbianhao = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.qichechepai = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.qicheleixing = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.pinpai = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.xinghao = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.nianfen = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.rizujin = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.yingfujiage = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.qichetupian = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.zucheshijian = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.cheshangzhanghao = true;
						}
						if(((json.yonghuzhanghao!=''&&json.yonghuzhanghao) || json.yonghuzhanghao==0) && this.$storage.get("role")!="管理员"){
							this.ruleForm.yonghuzhanghao = json.yonghuzhanghao
							this.ro.yonghuzhanghao = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.yonghuzhanghao = true;
						}
						if(((json.yonghuxingming!=''&&json.yonghuxingming) || json.yonghuxingming==0) && this.$storage.get("role")!="管理员"){
							this.ruleForm.yonghuxingming = json.yonghuxingming
							this.ro.yonghuxingming = true;
						}
						if(this.$storage.get("role")!="管理员") {
							this.ro.yonghuxingming = true;
						}
					} else {
						this.$message.error(data.msg);
					}
				});
			
			},
			// 多级联动参数

			info(id) {
				this.$http({
					url: `zulindingdan/info/${id}`,
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
					this.ruleForm.ispay = '未支付'
					if(this.ruleForm.dingdanbianhao) {
						this.ruleForm.dingdanbianhao = String(this.ruleForm.dingdanbianhao)
					}
					this.ruleForm.yingfujiage = this.yingfujiage
					if(this.ruleForm.qichetupian!=null) {
						this.ruleForm.qichetupian = this.ruleForm.qichetupian.replace(new RegExp(this.$base.url,"g"),"");
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
								url: `zulindingdan/${!this.ruleForm.id ? "save" : "update"}`,
								method: "post",
								data: this.ruleForm
							}).then(async ({ data }) => {
								if (data && data.code === 0) {
									this.$message({
										message: "操作成功",
										type: "success",
										duration: 1500,
										onClose: () => {
											if(this.isAuth('zulindingdan','支付')&&this.type=='cross') {
												this.$confirm('是否跳转支付？').then(_ => {
													this.parent.showFlag = true;
													this.parent.addOrUpdateFlag = false;
													this.parent.zulindingdanCrossAddOrUpdateFlag = false;
													this.$router.push('/zulindingdan')
												}).catch(_ => {
													this.parent.showFlag = true;
													this.parent.addOrUpdateFlag = false;
													this.parent.zulindingdanCrossAddOrUpdateFlag = false;
													this.parent.search();
													this.parent.contentStyleChange();
												});
											}else {
												this.parent.showFlag = true;
												this.parent.addOrUpdateFlag = false;
												this.parent.zulindingdanCrossAddOrUpdateFlag = false;
												this.parent.search();
												this.parent.contentStyleChange();
											}
											
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
				this.parent.zulindingdanCrossAddOrUpdateFlag = false;
				this.parent.contentStyleChange();
			},
			qichetupianUploadChange(fileUrls) {
				this.ruleForm.qichetupian = fileUrls;
			},
		}
	};
</script>
<style lang="css" scoped>
	.addEdit-block {
		padding: 30px;
	}
	.add-update-preview {
		padding: 40px 30px;
		background: none;
		border-color: #eee;
		border-width: 0px 0 0;
		border-style: solid;
	}
	.amap-wrapper {
		width: 100%;
		height: 500px;
	}
	
	.search-box {
		position: absolute;
	}
	
	.el-date-editor.el-input {
		width: auto;
	}
	.add-update-preview /deep/ .el-form-item {
		border: 0px solid #eee;
		padding: 0;
		margin: 0 0 26px 0;
		display: inline-block;
		width: 49%;
	}
	.add-update-preview .el-form-item /deep/ .el-form-item__label {
		padding: 0 10px 0 0;
		color: #6e6e6e;
		font-weight: 500;
		width: 180px;
		font-size: 15px;
		line-height: 40px;
		text-align: right;
	}
	
	.add-update-preview .el-form-item /deep/ .el-form-item__content {
		margin-left: 180px;
	}
	.add-update-preview .el-form-item span.text {
		padding: 0 10px;
		color: #666;
		background: none;
		font-weight: 500;
		display: inline-block;
		font-size: 15px;
		line-height: 40px;
		min-width: 100%;
	}
	
	.add-update-preview .el-input {
		width: 100%;
	}
	.add-update-preview .el-input /deep/ .el-input__inner {
		border: 1px solid #E8E8E8;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		width: 100%;
		font-size: 15px;
		min-width: 50%;
		height: 40px;
	}
	.add-update-preview .el-input /deep/ .el-input__inner[readonly="readonly"] {
		border: 0px solid #ccc;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		background: none;
		width: auto;
		font-size: 15px;
		height: 40px;
	}
	.add-update-preview .el-input-number {
		text-align: left;
		width: 100%;
	}
	.add-update-preview .el-input-number /deep/ .el-input__inner {
		text-align: left;
		border: 1px solid #E8E8E8;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		width: 100%;
		font-size: 15px;
		min-width: 50%;
		height: 40px;
	}
	.add-update-preview .el-input-number /deep/ .is-disabled .el-input__inner {
		text-align: left;
		border: 0px solid #ccc;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 12px;
		color: #666;
		background: none;
		width: auto;
		font-size: 15px;
		height: 40px;
	}
	.add-update-preview .el-input-number /deep/ .el-input-number__decrease {
		display: none;
	}
	.add-update-preview .el-input-number /deep/ .el-input-number__increase {
		display: none;
	}
	.add-update-preview .el-select {
		width: 100%;
	}
	.add-update-preview .el-select /deep/ .el-input__inner {
		border: 1px solid #E8E8E8;
		border-radius: 0px;
		padding: 0 10px;
		color: #666;
		width: 100%;
		font-size: 15px;
		height: 40px;
	}
	.add-update-preview .el-select /deep/ .is-disabled .el-input__inner {
		border: 0;
		cursor: not-allowed;
		border-radius: 4px;
		padding: 0 10px;
		color: #666;
		background: none;
		width: auto;
		font-size: 15px;
		height: 34px;
	}
	.add-update-preview .el-date-editor {
		width: 100%;
	}
	.add-update-preview .el-date-editor /deep/ .el-input__inner {
		border: 1px solid #E8E8E8;
		border-radius: 0px;
		padding: 0 10px 0 30px;
		color: #666;
		background: #fff;
		width: 100%;
		font-size: 15px;
		height: 40px;
	}
	.add-update-preview .el-date-editor /deep/ .el-input__inner[readonly="readonly"] {
		border: 0;
		cursor: not-allowed;
		border-radius: 0px;
		padding: 0 10px 0 30px;
		color: #666;
		background: none;
		width: auto;
		font-size: 15px;
		height: 40px;
	}
	.add-update-preview .viewBtn {
		border: 1px solid #E8E8E8;
		cursor: pointer;
		border-radius: 0px;
		padding: 0 15px;
		margin: 0 20px 0 0;
		color: #666;
		background: #fff;
		width: auto;
		font-size: 15px;
		line-height: 34px;
		height: 34px;
		.iconfont {
			margin: 0 2px;
			color: #666;
			font-size: 16px;
			height: 34px;
		}
	}
	.add-update-preview .viewBtn:hover {
		opacity: 0.8;
	}
	.add-update-preview .downBtn {
		border: 1px solid #E8E8E8;
		cursor: pointer;
		border-radius: 0px;
		padding: 0 15px;
		margin: 0 20px 0 0;
		color: #666;
		background: #fff;
		width: auto;
		font-size: 15px;
		line-height: 34px;
		height: 34px;
		.iconfont {
			margin: 0 2px;
			color: #666;
			font-size: 16px;
			height: 34px;
		}
	}
	.add-update-preview .downBtn:hover {
		opacity: 0.8;
	}
	.add-update-preview .unBtn {
		border: 0;
		cursor: not-allowed;
		border-radius: 4px;
		padding: 0 0px;
		margin: 0 20px 0 0;
		outline: none;
		color: #999;
		background: none;
		width: auto;
		font-size: 15px;
		line-height: 40px;
		height: 40px;
		.iconfont {
			margin: 0 2px;
			color: #fff;
			display: none;
			font-size: 14px;
			height: 34px;
		}
	}
	.add-update-preview .unBtn:hover {
		opacity: 0.8;
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
	
	.add-update-preview /deep/ .upload .upload-img {
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
	.add-update-preview /deep/ .el-upload__tip {
		color: #666;
		font-size: 15px;
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
	.add-update-preview .el-textarea /deep/ .el-textarea__inner[readonly="readonly"] {
				border: 0;
				cursor: not-allowed;
				border-radius: 0px;
				padding: 12px;
				color: #666;
				background: none;
				width: auto;
				font-size: 15px;
				min-width: 400px;
				height: auto;
			}
	.add-update-preview .el-form-item.btn {
		padding: 0;
		margin: 20px 0 0;
		.btn1 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #1a3194;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn1:hover {
			opacity: 0.8;
		}
		.btn2 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #1fc3cb;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 34px;
			}
		}
		.btn2:hover {
			opacity: 0.8;
		}
		.btn3 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #32c67a;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn3:hover {
			opacity: 0.8;
		}
		.btn4 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #b3b3b3;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn4:hover {
			opacity: 0.8;
		}
		.btn5 {
			border: 0px solid #ccc;
			cursor: pointer;
			border-radius: 4px;
			padding: 0 10px;
			margin: 0 10px 0 0;
			color: #fff;
			background: #156286;
			width: auto;
			font-size: 16px;
			min-width: 110px;
			height: 40px;
			.iconfont {
				margin: 0 2px;
				color: #fff;
				display: none;
				font-size: 14px;
				height: 40px;
			}
		}
		.btn5:hover {
			opacity: 0.8;
		}
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
	.add-update-preview /deep/ .el-form-item {
		display: flex;
		align-items: center;
		margin-bottom: 22px;
		width: 100%;
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
	.add-update-preview .el-select,
	.add-update-preview .el-input-number,
	.add-update-preview .el-date-editor {
		width: 100%;
	}
	.add-update-preview .el-input /deep/ .el-input__inner,
	.add-update-preview .el-select /deep/ .el-input__inner,
	.add-update-preview .el-input-number /deep/ .el-input__inner,
	.add-update-preview .el-date-editor /deep/ .el-input__inner {
		width: 100%;
		font-size: 15px;
		height: 40px;
		padding-left: 0 !important;
	}
	/* 隐藏el-upload的图片列表缩略图 */
	::v-deep .el-upload-list--picture-card {
		display: none !important;
	}
	/* 隐藏el-upload图片列表中的删除提示 */
	::v-deep .el-icon-close-tip {
		display: none !important;
	}
</style>
