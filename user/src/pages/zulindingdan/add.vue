<template>
	<div class="add-update-preview">
		<el-form
			class="add-update-form"
			ref="ruleForm"
			:model="ruleForm"
			:rules="rules"
			label-width="80px"
			>
			<el-form-item class="add-item" label="订单编号" prop="dingdanbianhao">
				<el-input v-model="ruleForm.dingdanbianhao" placeholder="订单编号" disabled></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="汽车车牌" prop="qichechepai">
				<el-input v-model="ruleForm.qichechepai" 
					placeholder="汽车车牌" clearable :disabled="true  ||ro.qichechepai"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="汽车类型" prop="qicheleixing">
				<el-input v-model="ruleForm.qicheleixing" 
					placeholder="汽车类型" clearable :disabled="true  ||ro.qicheleixing"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="品牌" prop="pinpai">
				<el-input v-model="ruleForm.pinpai" 
					placeholder="品牌" clearable :disabled="true  ||ro.pinpai"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="型号" prop="xinghao">
				<el-input v-model="ruleForm.xinghao" 
					placeholder="型号" clearable :disabled="true  ||ro.xinghao"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="年份" prop="nianfen">
				<el-input v-model="ruleForm.nianfen" 
					placeholder="年份" clearable :disabled="true  ||ro.nianfen"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="日租金" prop="rizujin">
				<el-input-number v-model="ruleForm.rizujin" placeholder="日租金" :disabled="true ||ro.rizujin"></el-input-number>
			</el-form-item>
			<div style="width: 100%;"></div>
			<el-form-item class="add-item" label="租车时间" prop="zucheshijian">
				<el-date-picker
					v-model="zuche_datetime"
					type="datetime"
					placeholder="选择租车日期时间"
					align="right"
					:picker-options="pickerOptions"
					@change="calculatePrice">
				</el-date-picker>
				<!-- 隐藏原始日期时间选择器，但保留它用于后端交互 -->
				<el-date-picker
					:disabled="true"
					value-format="yyyy-MM-dd HH:mm:ss"
					v-model="ruleForm.zucheshijian" 
					type="datetime"
					placeholder="租车时间"
					style="display: none;">
				</el-date-picker>
			</el-form-item>
			<el-form-item class="add-item" label="还车时间" prop="tianshu">
				<el-date-picker
					v-model="huanche_datetime"
					type="datetime"
					placeholder="选择还车日期时间"
					align="right"
					:picker-options="pickerOptions"
					@change="calculatePrice">
				</el-date-picker>
			</el-form-item>
			<el-form-item class="add-item" label="应付价格" prop="yingfujiage">
				<el-input v-model="yingfujiage" placeholder="应付价格" disabled></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="汽车图片" v-if="type!='cross' || (type=='cross' && !ro.qichetupian)" prop="qichetupian">
				<file-upload
					tip="点击上传汽车图片"
					action="file/upload"
					:limit="3"
					:multiple="true"
					:fileUrls="ruleForm.qichetupian?ruleForm.qichetupian:''"
					@change="qichetupianUploadChange"
					></file-upload>
			</el-form-item>
			<el-form-item class="add-item" v-else label="汽车图片" prop="qichetupian">
				<img v-if="ruleForm.qichetupian.substring(0,4)=='http'" class="upload-img" :key="'http-img'" :src="ruleForm.qichetupian.split(',')[0]">
				<img v-else class="upload-img" v-bind:key="index" v-for="(item,index) in ruleForm.qichetupian.split(',')" :src="baseUrl+item">
			</el-form-item>
			<el-form-item class="add-item" label="租车备注" prop="zuchebeizhu">
				<el-input v-model="ruleForm.zuchebeizhu" 
					placeholder="租车备注" clearable :disabled=" false  ||ro.zuchebeizhu"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="车商账号" prop="cheshangzhanghao">
				<el-input v-model="ruleForm.cheshangzhanghao" 
					placeholder="车商账号" clearable :disabled="true  ||ro.cheshangzhanghao"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="用户账号" prop="yonghuzhanghao">
				<el-input v-model="ruleForm.yonghuzhanghao" 
					placeholder="用户账号" clearable :disabled="true  ||ro.yonghuzhanghao"></el-input>
			</el-form-item>
			<el-form-item class="add-item" label="用户姓名" prop="yonghuxingming">
				<el-input v-model="ruleForm.yonghuxingming" 
					placeholder="用户姓名" clearable :disabled="true  ||ro.yonghuxingming"></el-input>
			</el-form-item>

			<el-form-item class="add-btn-item">
				<el-button class="submitBtn"  type="primary" @click="onSubmit">
					<span class="icon iconfont icon-kaitongfuwu"></span>
					<span class="text">更新</span>
				</el-button>
				<el-button class="closeBtn" @click="back()">
					<span class="icon iconfont icon-shanchu1"></span>
					<span class="text">取消</span>
				</el-button>
			</el-form-item>
		</el-form>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				id: '',
				baseUrl: '',
				xiaoshishu: '',
				zuche_datetime: new Date(),
				huanche_datetime: new Date(new Date().getTime() + 2 * 60 * 60 * 1000),
				pickerOptions: {
					shortcuts: [{
						text: '现在',
						onClick(picker) {
							picker.$emit('pick', new Date());
						}
					}, {
						text: '2小时后',
						onClick(picker) {
							const date = new Date();
							date.setTime(date.getTime() + 3600 * 1000 * 2);
							picker.$emit('pick', date);
						}
					}, {
						text: '1天后',
						onClick(picker) {
							const date = new Date();
							date.setTime(date.getTime() + 3600 * 1000 * 24);
							picker.$emit('pick', date);
						}
					}]
				},
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
				type: '',
				userTableName: localStorage.getItem('UserTableName'),
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
						{ validator: this.$validate.isNumber, trigger: 'blur' },
					],
					tianshu: [
						{ required: true, message: '小时数不能为空', trigger: 'blur' },
					],
					yingfujiage: [
						{ validator: this.$validate.isNumber, trigger: 'blur' },
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
				centerType: false,
			};
		},
		computed: {


			yingfujiage:{
				get: function () {
					if(this.zuche_datetime && this.huanche_datetime && this.ruleForm.rizujin) {
						let hours = this.getHoursDiff(this.zuche_datetime, this.huanche_datetime);
						return (hours * (this.ruleForm.rizujin / 24)).toFixed(2);
					}
					return '0';
				}
			},

		},
		components: {
		},
		created() {
			if(this.$route.query.centerType){
				this.centerType = true
			}
			//this.bg();
			let type = this.$route.query.type ? this.$route.query.type : '';
			this.init(type);
			this.baseUrl = this.$config.baseUrl;
			this.ruleForm.zucheshijian = this.getCurDateTime();
			this.zuche_datetime = new Date();
			this.huanche_datetime = new Date(new Date().getTime() + 2 * 60 * 60 * 1000);
			this.calculatePrice();
		},
		methods: {
			getMakeZero(s) {
				return s < 10 ? '0' + s : s;
			},
			// 下载
			download(file){
				window.open(`${file}`)
			},
			// 初始化
			init(type) {
				this.type = type;
				if(type=='cross'){
					var obj = JSON.parse(localStorage.getItem('crossObj'));
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
							
							if(obj[o] && this.zuche_datetime) {
								let hours = obj[o] * 24;
								this.xiaoshishu = hours;
								
								let zucheDate = new Date(this.zuche_datetime);
								this.huanche_datetime = new Date(zucheDate.getTime() + hours * 60 * 60 * 1000);
							}
							
							this.ro.tianshu = true;
							continue;
						}
						if(o=='yingfujiage'){
							this.ruleForm.yingfujiage = obj[o];
							this.ro.yingfujiage = true;
							continue;
						}
						if(o=='qichetupian'){
							this.ruleForm.qichetupian = obj[o].split(",")[0];
							this.ro.qichetupian = true;
							continue;
						}
						if(o=='zucheshijian'){
							this.ruleForm.zucheshijian = obj[o];
							if(obj[o]) {
								this.zuche_datetime = new Date(obj[o]);
							}
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
				}else if(type=='edit'){
					this.info()
				}
				// 获取用户信息
				this.$http.get(this.userTableName + '/session', {emulateJSON: true}).then(res => {
					if (res.data.code == 0) {
						var json = res.data.data;
						if((json.yonghuzhanghao!=''&&json.yonghuzhanghao) || json.yonghuzhanghao==0){
							this.ruleForm.yonghuzhanghao = json.yonghuzhanghao;
							this.ro.yonghuzhanghao = true;
						}
						if((json.yonghuxingming!=''&&json.yonghuxingming) || json.yonghuxingming==0){
							this.ruleForm.yonghuxingming = json.yonghuxingming;
							this.ro.yonghuxingming = true;
						}
					}
				});

				if (localStorage.getItem('raffleType') && localStorage.getItem('raffleType') != null) {
					localStorage.removeItem('raffleType')
					setTimeout(() => {
						this.onSubmit()
					}, 300)
				}
			},

			// 多级联动参数
			// 多级联动参数
			info() {
				this.$http.get(`zulindingdan/detail/${this.$route.query.id}`, {emulateJSON: true}).then(res => {
					if (res.data.code == 0) {
						this.ruleForm = res.data.data;
					}
				});
			},
			// 提交
			async onSubmit() {
				if(this.ruleForm.dingdanbianhao){
					this.ruleForm.dingdanbianhao = String(this.ruleForm.dingdanbianhao)
				}
				
				this.calculatePrice();
				
				this.ruleForm.yingfujiage = this.yingfujiage
				await this.$refs["ruleForm"].validate(async valid => {
					if(valid) {
						if(this.type=='cross'){
							var statusColumnName = localStorage.getItem('statusColumnName');
							var statusColumnValue = localStorage.getItem('statusColumnValue');
							if(statusColumnName && statusColumnName!='') {
								var obj = JSON.parse(localStorage.getItem('crossObj'));
								if(!statusColumnName.startsWith("[")) {
									for (var o in obj){
										if(o==statusColumnName){
											obj[o] = statusColumnValue;
										}
									}
									var table = localStorage.getItem('crossTable');
									await this.$http.post(table+'/update', obj).then(res => {});
								}
							}
						}


						await this.$http.post(`zulindingdan/${this.ruleForm.id?'update':this.centerType?'save':'add'}`, this.ruleForm).then(async res => {
							if (res.data.code == 0) {
								this.$message({
									message: '操作成功',
									type: 'success',
									duration: 1500,
									onClose: () => {
										if(this.isBackAuth('zulindingdan','支付')&&this.type=='cross'){
											this.$confirm('是否跳转支付？') .then(_ => {
												let jumpParams = {
													id: res.data.data,
													centerType: 1
												}
												this.$router.push({path: '/index/zulindingdanDetail', query: jumpParams});
											}).catch(_ => {
												this.$router.go(-1);
											});
										}else {
											this.$router.go(-1);
										}
										
										
									}
								});
							} else {
								this.$message({
									message: res.data.msg,
									type: 'error',
									duration: 1500
								});
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
				this.$router.go(-1);
			},
			qichetupianUploadChange(fileUrls) {
				this.ruleForm.qichetupian = fileUrls.replace(new RegExp(this.$config.baseUrl,"g"),"");
			},
			getHoursDiff(startTime, endTime) {
				let diff = 0;
				// 计算毫秒差并转换为小时
				diff = (endTime - startTime) / (60 * 60 * 1000);
				return parseFloat(diff.toFixed(2));
			},
			calculatePrice() {
				if(this.zuche_datetime && this.huanche_datetime) {
					// 同步日期时间到后端格式
					if(this.zuche_datetime) {
						let date = new Date(this.zuche_datetime);
						this.ruleForm.zucheshijian = this.formatDateTime(date);
					}
					
					// 计算小时差
					let hours = this.getHoursDiff(this.zuche_datetime, this.huanche_datetime);
					
					// 如果小时差为负数，提示用户
					if(hours <= 0) {
						this.$message({
							message: '还车时间必须晚于租车时间',
							type: 'warning',
							duration: 1500
						});
						// 自动修正：将还车时间设为租车时间后2小时
						this.huanche_datetime = new Date(new Date(this.zuche_datetime).getTime() + 2 * 60 * 60 * 1000);
						hours = 2;
					}
					
					// 将小时转换为天数，存入ruleForm.tianshu
					this.ruleForm.tianshu = (hours / 24).toFixed(4);
					// 小时数存入xiaoshishu
					this.xiaoshishu = hours;
				}
			},
			// 格式化日期时间为yyyy-MM-dd HH:mm:ss
			formatDateTime(date) {
				const year = date.getFullYear();
				const month = this.getMakeZero(date.getMonth() + 1);
				const day = this.getMakeZero(date.getDate());
				const hours = this.getMakeZero(date.getHours());
				const minutes = this.getMakeZero(date.getMinutes());
				const seconds = this.getMakeZero(date.getSeconds());
				return `${year}-${month}-${day} ${hours}:${minutes}:${seconds}`;
			},
		}
	};
</script>

<style rel="stylesheet/css" lang="css" scoped>
	.add-update-preview {
		padding: 0 16%;
		margin: 10px auto;
		background: none;
		width: 100%;
		position: relative;
		.add-update-form {
			padding: 20px 0;
			background: #fff;
			width: 100%;
			position: relative;
			.add-item.el-form-item {
				padding: 10px;
				margin: 0 0 10px;
				background: none;
				display: inline-block;
				width: 49%;
				/deep/ .el-form-item__label {
					padding: 0 10px 0 0;
					color: #4B3E39;
					font-weight: 500;
					width: 80px;
					font-size: 14px;
					line-height: 40px;
					text-align: right;
				}
				/deep/ .el-form-item__content {
					margin-left: 80px;
				}
				.el-input {
					width: 80%;
				}
				.el-input /deep/ .el-input__inner {
					border: 0;
					border-radius: 10px;
					padding: 0 12px;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-input /deep/ .el-input__inner[readonly="readonly"] {
					border: 0;
					cursor: not-allowed;
					border-radius: 10px;
					padding: 0 12px;
					box-shadow: 0 0 0px rgba(85, 85, 127, 0.5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-input-number /deep/ .el-input__inner {
					text-align: left;
					border: 0;
					border-radius: 10px;
					padding: 0 12px;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-input-number /deep/ .is-disabled .el-input__inner {
					text-align: left;
					border: 0;
					cursor: not-allowed;
					border-radius: 10px;
					padding: 0 12px;
					box-shadow: 0 0 0px rgba(85, 85, 127, 0.5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-input-number /deep/ .el-input-number__decrease {
					display: none;
				}
				.el-input-number /deep/ .el-input-number__increase {
					display: none;
				}
				.el-select {
					width: 80%;
				}
				.el-select /deep/ .el-input__inner {
					border: 0;
					border-radius: 10px;
					padding: 0 10px;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-select /deep/ .is-disabled .el-input__inner {
					border: 0;
					cursor: not-allowed;
					border-radius: 10px;
					padding: 0 10px;
					box-shadow: 0 0 0px rgba(85, 85, 127, 0.5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-date-editor {
					width: 80%;
				}
				.el-date-editor /deep/ .el-input__inner {
					border: 0;
					border-radius: 10px;
					padding: 0 10px 0 30px;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				.el-date-editor /deep/ .el-input__inner[readonly="readonly"] {
					border: 0;
					cursor: not-allowed;
					border-radius: 10px;
					padding: 0 10px 0 30px;
					box-shadow: 0 0 0px rgba(85, 85, 127, 0.5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100%;
					font-size: 14px;
					height: 40px;
				}
				/deep/ .el-upload--picture-card {
					background: transparent;
					border: 0;
					border-radius: 0;
					width: auto;
					height: auto;
					line-height: initial;
					vertical-align: middle;
				}
				/deep/ .upload .upload-img {
					border: 0px dashed rgba(64, 158, 255, 1);
					cursor: pointer;
					border-radius: 6px;
					color: #23469A;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100px;
					font-size: 32px;
					line-height: 100px;
					text-align: center;
					height: 100px;
				}
				/deep/ .el-upload-list .el-upload-list__item {
					border: 0px dashed rgba(64, 158, 255, 1);
					cursor: pointer;
					border-radius: 6px;
					color: #23469A;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100px;
					font-size: 32px;
					line-height: 100px;
					text-align: center;
					height: 100px;
					font-size: 14px;
					line-height: 1.8;
				}
				/deep/ .el-upload .el-icon-plus {
					border: 0px dashed rgba(64, 158, 255, 1);
					cursor: pointer;
					border-radius: 6px;
					color: #23469A;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 100px;
					font-size: 32px;
					line-height: 100px;
					text-align: center;
					height: 100px;
				}
				/deep/ .el-upload__tip {
					color: #838fa1;
					display: none;
				}
				.el-textarea /deep/ .el-textarea__inner {
					border: 0;
					border-radius: 10px;
					padding: 12px;
					box-shadow: 0 0 0px rgba(64, 158, 255, .5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 80%;
					font-size: 14px;
					height: 120px;
				}
				.el-textarea /deep/ .el-textarea__inner[readonly="readonly"] {
					border: 0;
					cursor: not-allowed;
					border-radius: 10px;
					padding: 12px;
					box-shadow: 0 0 0px rgba(85, 85, 127, 0.5);
					outline: none;
					color: #000;
					background: linear-gradient(135.00deg, rgb(232, 240, 255) 0%,rgb(244, 248, 253) 100%);
					width: 80%;
					font-size: 14px;
					height: 120px;
				}
				/deep/ .el-input__inner::placeholder {
					color: #6e6e6e;
					font-size: 14px;
				}
				/deep/ textarea::placeholder {
					color: #6e6e6e;
					font-size: 14px;
				}
				.editor {
					background-color: #fff;
					border-radius: 0;
					padding: 0;
					box-shadow: 0 0 0px rgba(75,223,201,.5);
					margin: 0;
					width: 100%;
					border-color: #ccc;
					border-width: 0;
					border-style: solid;
					height: auto;
				}
				.upload-img {
					width: 100px;
					height: 100px;
				}
				.viewBtn {
					border: 0;
					cursor: pointer;
					padding: 0 10px;
					margin: 0;
					color: #fff;
					display: inline-block;
					font-size: 14px;
					line-height: 30px;
					border-radius: 4px;
					outline: none;
					background: #23469A;
					width: auto;
					height: 30px;
				}
				.viewBtn:hover {
					color: #fff;
					background: #23469A;
				}
				.unviewBtn {
					border: 0;
					cursor: not-allowed;
					padding: 0 10px;
					margin: 0;
					color: #E8D6B0;
					display: inline-block;
					font-size: 14px;
					line-height: 30px;
					border-radius: 4px;
					outline: none;
					background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
					width: auto;
					height: 30px;
				}
				.unviewBtn:hover {
					color: #E8D6B0;
					background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
				}
			}
			.add-btn-item {
				padding: 0;
				margin: 0;
				.submitBtn {
					border: 0;
					cursor: pointer;
					border-radius: 0;
					padding: 0 15px;
					margin: 0 20px 0 0;
					outline: none;
					background: #23469A;
					display: inline-block;
					width: auto;
					font-size: 14px;
					line-height: 40px;
					height: 40px;
					.icon {
						color: rgba(255, 255, 255, 1);
					}
					.text {
						color: rgba(255, 255, 255, 1);
					}
				}
				.submitBtn:hover {
					opacity: 0.7;
					.icon {
						color: #fff;
					}
					.text {
						color: #fff;
					}
				}
				.closeBtn {
					border: 0px solid rgba(64, 158, 255, 1);
					cursor: pointer;
					border-radius: 4px;
					padding: 0 15px;
					margin: 0 20px 0 0;
					outline: none;
					background: #E8E8E8;
					display: inline-block;
					width: auto;
					font-size: 14px;
					line-height: 40px;
					height: 40px;
					.icon {
						color: #6E6E6E;
					}
					.text {
						color: #6E6E6E;
					}
				}
				.closeBtn:hover {
					opacity: 0.7;
					.icon {
						color: #6E6E6E;
					}
					.text {
						color: #6E6E6E;
					}
				}
			}
		}
	}
	.el-date-editor.el-input {
		width: auto;
	}
</style>
