<template>
	<div>
	<!--  -->
		<div class="breadcrumb-preview">
			<el-breadcrumb :separator="'>'">
				<el-breadcrumb-item class="item1" to="/"><i class="el-icon-s-home"></i> 首页</el-breadcrumb-item>
				<el-breadcrumb-item class="item2" v-if="centerType" :to="'/index/center'">个人中心</el-breadcrumb-item>
				<el-breadcrumb-item class="item2" v-for="(item, index) in breadcrumbItem" :key="index" :to="'/index/zulindingdan?centerType=' + (centerType?'1':'0')">{{item.name}}</el-breadcrumb-item>
				<el-breadcrumb-item class="item3">详情</el-breadcrumb-item>
			</el-breadcrumb>
		</div>
		<div class="detail-preview">
			<div class="attr">
				<div class="info">
					<div class="title-item">
						<div class="detail-title">
							{{detail.qichechepai}}
						</div>
					</div>
					<div class="item">
						<div class="lable">订单编号</div>
						<div class="text "  >{{detail.dingdanbianhao}}</div>
					</div>
					<div class="item">
						<div class="lable">汽车类型</div>
						<div class="text "  >{{detail.qicheleixing}}</div>
					</div>
					<div class="item">
						<div class="lable">品牌</div>
						<div class="text "  >{{detail.pinpai}}</div>
					</div>
					<div class="item">
						<div class="lable">型号</div>
						<div class="text "  >{{detail.xinghao}}</div>
					</div>
					<div class="item">
						<div class="lable">年份</div>
						<div class="text "  >{{detail.nianfen}}</div>
					</div>
					<div class="item">
						<div class="lable">日租金</div>
						<div class="text "  >{{detail.rizujin}}</div>
					</div>
					<div class="item">
						<div class="lable">天数</div>
						<div class="text "  >{{detail.tianshu}}</div>
					</div>
					<div class="item">
						<div class="lable">应付价格</div>
						<div class="text "  >{{detail.yingfujiage}}</div>
					</div>
					<div class="item">
						<div class="lable">租车时间</div>
						<div class="text "  >{{detail.zucheshijian}}</div>
					</div>
					<div class="item">
						<div class="lable">租车备注</div>
						<div class="text "  >{{detail.zuchebeizhu}}</div>
					</div>
					<div class="item">
						<div class="lable">车商账号</div>
						<div class="text "  >{{detail.cheshangzhanghao}}</div>
					</div>
					<div class="item">
						<div class="lable">用户账号</div>
						<div class="text "  >{{detail.yonghuzhanghao}}</div>
					</div>
					<div class="item">
						<div class="lable">用户姓名</div>
						<div class="text "  >{{detail.yonghuxingming}}</div>
					</div>
					<div class="item" v-if="btnAuth('zulindingdan','支付')">
						<div class="lable">是否支付</div>
						<div class="text">{{detail.ispay=='已支付'?'已支付':'未支付'}}</div>
					</div>
					<div class="btn_box">
						<el-button class="editBtn" v-if="btnAuth('zulindingdan','修改')" @click="editClick">修改</el-button>
						<el-button class="delBtn" v-if="btnAuth('zulindingdan','删除')" @click="delClick">删除</el-button>
						<el-button class="reserveBtn" 
							v-if="btnAuth('zulindingdan','归还汽车')" 
							@click="onAcross('haichejilu','否','是','[1]','已归还')" type="warning">
							归还汽车
						</el-button>
						<el-button class="reserveBtn" 
							v-if="btnAuth('zulindingdan','订单取消')" 
							@click="onAcross('dingdanquxiao','否','否','[1]','已取消')" type="warning">
							订单取消
						</el-button>
						<el-button class="reserveBtn" 
							v-if="btnAuth('zulindingdan','故障上报')" 
							@click="onAcross('guzhangshangbao','否','是','','')" type="warning">
							故障上报
						</el-button>
						<el-button class="reserveBtn" 
							v-if="btnAuth('zulindingdan','事故上报')" 
							@click="onAcross('shigushangbao','否','是','','')" type="warning">
							事故上报
						</el-button>
						<el-button class="reserveBtn" 
							v-if="btnAuth('zulindingdan','车辆保险')" 
							@click="onAcross('cheliangbaoxian','否','是','','')" type="warning">
							车辆保险
						</el-button>
						<el-button class="payBtn" v-if="(detail.ispay=='未支付'||!detail.ispay)&&btnAuth('zulindingdan','支付')" type="warning" size="small" @click="payClick">支付</el-button>
					</div>
				</div>
			</div>
		
			<div class="swiper3" v-if="detailBanner.length">
				<div class="big">
					<img id="big" :src="swiperBigUrl" class="image">
				</div>
				<div class="samll">
					<div class="swiper3-small-item" v-for="item in detailBanner" :key="item.id">
						<img v-if="item.substr(0,4)=='http'" :src="item" @click="swiperClick3(item)" class="image">
						<img v-else :src="baseUrl + item" @click="swiperClick3(baseUrl + item)" class="image">
					</div>
				</div>
			</div>


		

			<el-tabs class="detail-tabs" v-model="activeName" type="border-card" v-if="tabsNum>0" >
			</el-tabs>

		</div>
		<div class="share_view">
		</div>
	</div>
</template>

<script>
	import axios from 'axios'
	import Swiper from "swiper";
	export default {
		//数据集合
		data() {
			return {
				tablename: 'zulindingdan',
				baseUrl: '',
				breadcrumbItem: [
					{
						name: '租赁订单'
					}
				],
				title: '',
				detailBanner: [],
				userid: localStorage.getItem('frontUserid'),
				id: 0,
				detail: {},
				tabsNum: 0,
				activeName: 'first',
				total: 1,
				pageSize: 10,
				totalPage: 1,
				buynumber: 1,
				centerType: false,
				storeupType: false,
				shareUrl: location.href,
				swiperBigUrl: null,
				sensitiveWordsArr: [],
			}
		},
		created() {
			if(this.$route.query.centerType&&this.$route.query.centerType!=0) {
				this.centerType = true
			}
			if(this.$route.query.storeupType&&this.$route.query.storeupType!=0) {
				this.storeupType = true
			}
			
			this.init();
		},
		mounted() {
		},
		//方法集合
		methods: {
			swiperClick3(src) {
				this.swiperBigUrl = src
			},
			init() {
				this.id = this.$route.query.id
				this.baseUrl = this.$config.baseUrl;
				this.$http.get(this.tablename + '/detail/'  + this.id, {}).then(res => {
					if (res.data.code == 0) {
						this.detail = res.data.data;
						// 获取数据库敏感词
						this.getSensitiveWords()
						this.title = this.detail.qichechepai;
						if(this.detail.qichetupian) {
							this.detailBanner = this.detail.qichetupian.split(",w").length>1?[this.detail.qichetupian]:this.detail.qichetupian.split(',');
						}
						this.$forceUpdate();
						if(localStorage.getItem('frontToken')){
						}

					}
					if (this.detailBanner.length) {
						if (this.detailBanner[0].substr(0,4)=='http') {
							this.swiperBigUrl = this.detailBanner[0]
						} else {
							this.swiperBigUrl = this.baseUrl + this.detailBanner[0]
						}
					}
				});
			},
			getSensitiveWords(){
				this.$http.get('sensitivewords/detail/1').then(rs=>{
					this.sensitiveWordsArr = rs.data.data.content.split(',')
				})
			},
			async onAcross(acrossTable,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue,type=1){
				if(crossOptPay=='是'&&this.detail.ispay!='已支付') {
					this.$message.error('请支付完成再操作');
					return;
				}
				localStorage.setItem('crossTable',`zulindingdan`);
				localStorage.setItem('crossObj', JSON.stringify(this.detail));
				localStorage.setItem('statusColumnName',statusColumnName);
				localStorage.setItem('statusColumnValue',statusColumnValue);
				localStorage.setItem('tips',tips);
				if(statusColumnName!=''&&!statusColumnName.startsWith("[")) {
					var obj = JSON.parse(localStorage.getItem('crossObj'));
					for (var o in obj){
						if(o==statusColumnName && obj[o]==statusColumnValue){
							this.$message({
								type: 'error',
								message: tips,
								duration: 1500
							});
							return
						}
					}
				}
				this.$router.push({path: '/index/' + acrossTable + 'Add', query: {type: 'cross'}});
			},
			payClick(){
				localStorage.setItem('paytable','zulindingdan')
				localStorage.setItem('payObject',JSON.stringify(this.detail))
				this.$router.push({path:'/index/pay'})
			},
			curChange(page) {
				this.getDiscussList(page);
			},
			prevClick(page) {
				this.getDiscussList(page);
			},
			nextClick(page) {
				this.getDiscussList(page);
			},
			sizeChange(size){
				this.pageSize = size
				this.getDiscussList(1);
			},
			// 权限判断
			btnAuth(tableName,key){
				if(this.centerType){
					return this.isBackAuth(tableName,key)
				}else{
					return this.isAuth(tableName,key)
				}
			},
			// 修改
			editClick(){
				this.$router.push(`/index/zulindingdanAdd?type=edit&&id=${this.detail.id}`);
			},
			// 删除
			async delClick(){
				await this.$confirm('是否删除此租赁订单？') .then(_ => {
					this.$http.post('zulindingdan/delete', [this.detail.id]).then(async res => {
						if (res.data.code == 0) {
							this.$message({
								type: 'success',
								message: '删除成功!',
								duration: 1500,
								onClose: () => {
									history.back()
								}
							});
						}
					});
				}).catch(_ => {});
			},
			getDiscussCount(){
				this.$http({
					url: `discuss${this.tablename}/count`,
					method: "get",
					params: {
						refid: this.id
					}
				}).then(({ data }) => {
					if (data && data.code === 0) {
						this.total = data.data;
					}
				}).catch(_ => {});
			},
			// 下载
			download(file ){
				if(!file) {
					this.$message({
						type: 'error',
						message: '文件不存在',
						duration: 1500,
					});
					return;
				}
				let arr = file.replace(new RegExp('upload/', "g"), "")
				axios.get(this.baseUrl + '/file/download?fileName=' + arr, {
					headers: {
						token: localStorage.getItem("frontToken")
					},
					responseType: "blob"
				}).then(({
					data
				}) => {
					const binaryData = [];
					binaryData.push(data);
					const objectUrl = window.URL.createObjectURL(new Blob(binaryData, {
						type: 'application/pdf;chartset=UTF-8'
					}))
					const a = document.createElement('a')
					a.href = objectUrl
					a.download = arr
					// a.click()
					// 下面这个写法兼容火狐
					a.dispatchEvent(new MouseEvent('click', {
						bubbles: true,
						cancelable: true,
						view: window
					}))
					window.URL.revokeObjectURL(data)
				},err=>{
					axios.get((location.href.split(this.$config.name).length>1 ? location.href.split(this.$config.name)[0] :'') + this.$config.name + '/file/download?fileName=' + arr, {
						headers: {
							token: localStorage.getItem("frontToken")
						},
						responseType: "blob"
					}).then(({
						data
					}) => {
						const binaryData = [];
						binaryData.push(data);
						const objectUrl = window.URL.createObjectURL(new Blob(binaryData, {
							type: 'application/pdf;chartset=UTF-8'
						}))
						const a = document.createElement('a')
						a.href = objectUrl
						a.download = arr
						// a.click()
						// 下面这个写法兼容火狐
						a.dispatchEvent(new MouseEvent('click', {
							bubbles: true,
							cancelable: true,
							view: window
						}))
						window.URL.revokeObjectURL(data)
					})
				})
			},
		},
		components: {
		}
	}
</script>

<style rel="stylesheet/css" lang="css" scoped>
	.breadcrumb-preview {
		padding: 0 16%;
		margin: 15px 0 0 0;
		background: transparent;
		width: 100%;
		box-shadow: none;
		position: relative;
	}
	
	.detail-preview {
		padding: 0 16%;
		margin: 0px auto;
		display: flex;
		width: 100%;
		position: relative;
		flex-wrap: wrap;
		.attr {
			padding: 10px 0;
			background: #fff;
			display: flex;
			width: 100%;
			position: relative;
			order: 3;
			.info {
				padding: 10px 0;
				margin: 0 0 0 10px;
				background: #fff;
				flex: 1;
				display: flex;
				justify-content: space-between;
				flex-wrap: wrap;
				.title-item {
					padding: 10px;
					margin: 0 0 10px 0;
					background: linear-gradient(0.00deg, rgb(145, 177, 255) 0%,rgb(255, 255, 255) 24.428%,rgb(255, 255, 255) 100%);
					display: flex;
					width: 48%;
					border-color: #25489C80;
					border-width: 0 0 2px;
					justify-content: space-between;
					align-items: center;
					border-style: solid;
					.detail-title {
						color: #333;
						font-size: 16px;
					}
				}
				.item {
					padding: 10px;
					margin: 0 0 10px 0;
					background: linear-gradient(0.00deg, rgb(145, 177, 255) 0%,rgb(255, 255, 255) 24.428%,rgb(255, 255, 255) 100%);
					display: flex;
					width: 48%;
					border-color: #25489C80;
					border-width: 0 0 2px;
					justify-content: flex-start;
					align-items: center;
					border-style: solid;
					.lable {
						padding: 0 10px;
						color: #000;
						white-space: nowrap;
						width: auto;
						font-size: 13px;
						line-height: 24px;
						text-align: left;
						height: auto;
					}
					.text {
						padding: 0 10px;
						color: #25489C;
						word-break: break-all;
						flex: 1;
						font-size: 14px;
						line-height: 24px;
						height: auto;
					}
					.price {
						color: #f00;
					}
					.bold {
						font-weight: bold;
					}
					.link {
						cursor: pointer;
						text-decoration: underline;
					}
				}
				.btn_box {
					padding: 10px 0;
					display: flex;
					width: 100%;
					align-items: center;
					flex-wrap: wrap;
				}
				.editBtn {
					border: 0;
					cursor: pointer;
					border-radius: 0;
					padding: 0 10px;
					margin: 0 5px 0 0;
					outline: none;
					color: #fff;
					background: #007ED0;
					width: auto;
					font-size: 14px;
					line-height: 40px;
					height: 40px;
				}
				.editBtn:hover {
					opacity: 0.7;
				}
				.delBtn {
					border: 0;
					cursor: pointer;
					border-radius: 0;
					padding: 0 10px;
					margin: 0 5px 0 0;
					outline: none;
					color: #fff;
					background: #218BD0;
					width: auto;
					font-size: 14px;
					line-height: 40px;
					height: 40px;
				}
				.delBtn:hover {
					opacity: 0.7;
				}
				.payBtn {
					border: 0;
					cursor: pointer;
					border-radius: 0;
					padding: 0 10px;
					margin: 0 5px 0 0;
					outline: none;
					color: #fff;
					background: #024DC7;
					width: auto;
					font-size: 14px;
					line-height: 40px;
					height: 40px;
				}
				.payBtn:hover {
					opacity: 0.7;
				}
			}
		}
		.swiper3 {
			background: none;
			display: flex;
			width: calc(100% - 200px);
			align-items: flex-start;
			height: auto;
			order: 1;
			.big {
				border: 0px solid red;
				margin: 0 20px 0 0;
				background: #fff;
				width: 100%;
				position: relative;
				height: 400px;
				img {
					object-fit: cover;
					display: block;
					width: 100%;
					height: 100%;
				}
			}
			.samll {
				padding: 0 0 100px;
				flex-direction: column;
				background: none;
				display: flex;
				width: 0;
				height: 400px;
				.swiper3-small-item {
					border: 0px solid red;
					margin: 0 0 5px;
					background: none;
					flex: 1;
					width: 200px;
					position: relative;
					height: 20%;
					img {
						object-fit: cover;
						display: block;
						width: 100%;
						height: 100%;
					}
				}
			}
		}
		.detail-tabs {
			border: 0px solid #DCDFE6;
			box-shadow: 0 0px 0px 0 rgba(0, 0, 0, .1);
			background: #fff;
			width: 100%;
			order: 6;
			& /deep/ .el-tabs__header .el-tabs__nav-wrap {
				margin-bottom: 0;
			}
			/deep/ .el-tabs__header {
				padding: 10px;
				margin: 0;
				background: #fff;
				border-color: #23469A;
				border-width: 0 0 2px;
				border-style: solid;
			}
			
			/deep/ .el-tabs__header .el-tabs__item {
				border: 3px solid rgb(35, 70, 154);
				padding: 0 20px;
				margin: 0 10px 0 0;
				color: #23469A;
				font-weight: 500;
				display: inline-block;
				font-size: 14px;
				line-height: 40px;
				border-radius: 0 0 30px 0;
				background: linear-gradient(180.00deg, rgb(145, 177, 255),rgb(222, 229, 253) 98.473%);
				position: relative;
				list-style: none;
				height: auto;
			}
			
			/deep/ .el-tabs__header .el-tabs__item:hover {
				border: 3px solid rgb(232, 214, 176);
				border-radius: 0 0 30px 0;
				margin: 0 10px 0 0;
				color: #AD8B44;
				background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
				line-height: 40px;
				height: auto;
			}
			
			/deep/ .el-tabs__header .el-tabs__item.is-active {
				border: 3px solid rgb(232, 214, 176);
				border-radius: 0 0 30px 0;
				margin: 0 10px 0 0;
				color: #AD8B44;
				background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
				line-height: 40px;
				height: auto;
			}
			
			/deep/ .el-tabs__content {
				padding: 15px;
			}
		}
	}
	.share_view{
		box-shadow: 0 1px 6px rgba(0,0,0,.3);
		z-index: 11;
		bottom: 20%;
		background: #fff;
		position: fixed;
		right: 0;
		.share:last-of-type{
			border:none;
		}
	}
</style>
