<template>
	<div>
		<div class="breadcrumb-preview">
			<div class="breadcrumb-container">
				<el-breadcrumb :separator="'>'">
					<el-breadcrumb-item class="item1" to="/"><i class="el-icon-s-home"></i> 首页</el-breadcrumb-item>
					<el-breadcrumb-item class="item2" v-if="centerType" :to="'/index/center'">个人中心</el-breadcrumb-item>
					<el-breadcrumb-item class="item2" v-for="(item, index) in breadcrumbItem" :key="index">{{item.name}}</el-breadcrumb-item>
				</el-breadcrumb>
			</div>
		</div>
		<div class="list-preview">
			<div class="category-3">
				<div class="item" :class="swiperIndex == '-1' ? 'active' : ''" @click="getList(1, '全部')" :plain="isPlain">
					<div class="text">全部</div>
				</div>
				<div class="item" :class="swiperIndex == index ? 'active' : ''" v-for="(item, index) in fenlei" :key="index" @click="getList(1, item[feileiColumn], 'btn' + index)" :ref="'btn' + index" plain>
					<img v-if="item.image" :src="baseUrl + (item.image?item.image.split(',')[0]:'')">
					<div class="text">{{item[feileiColumn]}}</div>
				</div>
			</div>
			<el-form :inline="true" :model="formSearch" class="list-form-pv">
				<el-form-item class="list-item">
					<div class="lable">租赁标题：</div>
					<el-input v-model="formSearch.zulinbiaoti" placeholder="租赁标题" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-form-item class="list-item">
					<div class="lable">汽车车牌：</div>
					<el-input v-model="formSearch.qichechepai" placeholder="汽车车牌" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-form-item class="list-item">
					<div class="lable">地区：</div>
					<el-input v-model="formSearch.diqu" placeholder="地区" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-form-item class="list-item">
					<div class="lable">品牌：</div>
					<el-input v-model="formSearch.pinpai" placeholder="品牌" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-form-item class="list-item">
					<div class="lable">型号：</div>
					<el-input v-model="formSearch.xinghao" placeholder="型号" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-form-item class="list-item">
					<div class="lable">年份：</div>
					<el-input v-model="formSearch.nianfen" placeholder="年份" @keydown.enter.native="getList(1, curFenlei)" clearable></el-input>
				</el-form-item>
				<el-button class="list-search-btn" v-if=" true " type="primary" @click="getList(1, curFenlei)">
					查询
				</el-button>
				<el-button class="list-add-btn" v-if="btnAuth('qichechuzu','新增')" type="primary" @click="add('/index/qichechuzuAdd')">
					添加
				</el-button>
			</el-form>
			<div class="select2">
				<div class="select2-list" v-for="(item,index) in selectOptionsList" :key="item">
					<div class="label">{{item.name}}：</div>
					<div class="item-body">
						<div class="item" @click="selectClick2(item,-1)" :class="item.check ==-1 ? 'active' : ''">全部</div>
						<div class="item" @click="selectClick2(item,index1)" :class="item.check == index1 ? 'active' : ''" v-for="item1,index1 in item.list" :key="item1">{{item1}}</div>
					</div>
				</div>
			</div>
			<div class="sort_view">
				<el-button class="click-sort-btn" @click="sortClick('clicknum')">
					<span class="icon iconfont icon-liulan12" v-if="sortType!='clicknum'"></span>
					<span class="icon iconfont icon-jiantou35" v-else-if="sortType=='clicknum'&&sortOrder=='desc'"></span>
					<span class="icon iconfont icon-jiantou36" v-else-if="sortType=='clicknum'&&sortOrder=='asc'"></span>
					<span class="text">点击量</span>
				</el-button>
				<el-button class="collect-sort-btn" @click="sortClick('storeupnum')">
					<span class="icon iconfont icon-shoucang12" v-if="sortType!='storeupnum'"></span>
					<span class="icon iconfont icon-jiantou35" v-else-if="sortType=='storeupnum'&&sortOrder=='desc'"></span>
					<span class="icon iconfont icon-jiantou36" v-else-if="sortType=='storeupnum'&&sortOrder=='asc'"></span>
					<span class="text">收藏数</span>
				</el-button>
			</div>
			<div class="list">
				<div class="index-pv1">
					<div v-for="(item, index) in dataList" :key="index" @click.stop="toDetail(item)" class="animation-box">
						<img class="image" @click.stop="imgPreView(getImageUrl(item.qichetupian))" v-if="item.qichetupian && item.qichetupian.substr(0,4)=='http'&& item.qichetupian.split(',w').length>1" :src="getImageUrl(item.qichetupian)" />
						<img class="image" @click.stop="imgPreView(getImageUrl(item.qichetupian.split(',')[0]))" v-else-if="item.qichetupian && item.qichetupian.substr(0,4)=='http'" :src="getImageUrl(item.qichetupian.split(',')[0])" />
						<img class="image" @click.stop="imgPreView(getImageUrl(baseUrl + (item.qichetupian?item.qichetupian.split(',')[0]:'')))" v-else :src="getImageUrl(baseUrl + (item.qichetupian?item.qichetupian.split(',')[0]:''))" />
						
						<div class="name">{{item.zulinbiaoti}}</div>
						
						<div class="info-container">
							<div class="info-item">
								<span class="icon time-icon iconfont icon-shijian21"></span>
								<span class="text">{{item.addtime.split(' ')[0]}}</span>
							</div>
							<div class="info-item">
								<span class="icon publisher-icon iconfont icon-geren16"></span>
								<span class="text">{{item.cheshangzhanghao}}</span>
							</div>
							<div class="info-item">
								<span class="icon collect-icon iconfont icon-shoucang10"></span>
								<span class="text">{{item.storeupnum || 0}}</span>
							</div>
							<div class="info-item" v-if="item.clicknum">
								<span class="icon view-icon iconfont icon-chakan9"></span>
								<span class="text">{{item.clicknum}}</span>
							</div>
						</div>
						
						<div class="status-tag" :class="item.zhuangtai === '已租赁' ? 'rented' : 'available'">
							{{item.zhuangtai || '待租赁'}}
						</div>
					</div>
				</div>
			</div>

	
			<el-pagination
				background
				id="pagination"
				class="pagination"
				:pager-count="7"
				:page-size="pageSize"
				prev-text="<"
				next-text=">"
				:hide-on-single-page="true"
				:layout='["prev","pager","next","jumper"].join()'
				:total="total"
				:page-sizes="pageSizes"
				@current-change="curChange"
				@size-change="sizeChange"
				@prev-click="prevClick"
				@next-click="nextClick"
				></el-pagination>
		</div>
		<el-dialog title="预览图" :visible.sync="previewVisible" width="50%">
			<img :src="previewImg" alt="" style="width: 100%;">
		</el-dialog>
	</div>
</template>
<script>
	export default {
		//数据集合
		data() {
			return {
				selectIndex2: 0,
				selectOptionsList: [],
				layouts: '',
				swiperIndex: -1,
				baseUrl: '',
				breadcrumbItem: [
					{
						name: '汽车租赁'
					}
				],
				formSearch: {
					zulinbiaoti: '',
					qichechepai: '',
					diqu: '',
					pinpai: '',
					xinghao: '',
					nianfen: '',
					zhuangtai: '',
				},
				fenlei: [],
				feileiColumn: '',
				dataList: [],
				total: 1,
				pageSize: 10,
				pageSizes: [],
				totalPage: 1,
				curFenlei: '全部',
				isPlain: false,
				indexQueryCondition: '',
				zhuangtaiOptions: [],
				timeRange: [],
				centerType:false,
				previewImg: '',
				previewVisible: false,
				sortType: 'id',
				sortOrder: 'desc',
			}
		},
		async created() {
			if(this.$route.query.centerType&&this.$route.query.centerType!=0){
				this.centerType = true
			}
			this.baseUrl = this.$config.baseUrl;
			this.zhuangtaiOptions = '已租赁,待租赁'.split(',');
			this.selectOptionsList.push({name:'状态',list:this.zhuangtaiOptions,tableName: 'zhuangtai',check: -1})
			await this.getFenlei();
			this.getList(1, '全部');
		},
		watch:{
			$route(newValue){
				this.getList(1, newValue.query.homeFenlei);
			}
		},
		//方法集合
		methods: {
			selectClick2(row,index) {
				row.check = index
				if(index == -1){
					this.formSearch[row.tableName] = ''
				}else {
					this.formSearch[row.tableName] = row.list[index]
				}
				this.getList()
			},
			add(path) {
				let query = {}
				if(this.centerType){
					query.centerType = 1
				}
				this.$router.push({path: path,query:query});
			},
			async getFenlei() {
				await this.$http.get('qicheleixing/list',{params: {sort: 'id',order: 'asc'}}).then(res => {
					if (res.data.code == 0) {
						this.fenlei = res.data.data.list
					}
				});
				this.feileiColumn = 'qicheleixing'
			},
			getList(page, fenlei, ref = '') {
				if(fenlei == '全部') this.swiperIndex = -1;
				for(let i=0;i<this.fenlei.length;i++) {
					if(fenlei == this.fenlei[i][this.feileiColumn]) {
						this.swiperIndex = i;
						break;
					}
				}
				if(fenlei){
					this.curFenlei = fenlei;
				}
				let params = {
					page,
					limit: this.pageSize,
				};
				let searchWhere = {};
				if (this.formSearch.zulinbiaoti != '') searchWhere.zulinbiaoti = '%' + this.formSearch.zulinbiaoti + '%';
				if (this.formSearch.qichechepai != '') searchWhere.qichechepai = '%' + this.formSearch.qichechepai + '%';
				if (this.formSearch.diqu != '') searchWhere.diqu = '%' + this.formSearch.diqu + '%';
				if (this.formSearch.pinpai != '') searchWhere.pinpai = '%' + this.formSearch.pinpai + '%';
				if (this.formSearch.xinghao != '') searchWhere.xinghao = '%' + this.formSearch.xinghao + '%';
				if (this.formSearch.nianfen != '') searchWhere.nianfen = '%' + this.formSearch.nianfen + '%';
				if (this.formSearch.zhuangtai != '') searchWhere.zhuangtai = this.formSearch.zhuangtai;
				if (this.curFenlei != '全部') searchWhere.qicheleixing = this.curFenlei;
				let user = JSON.parse(localStorage.getItem('sessionForm'))
				if (this.sortType) searchWhere.sort = this.sortType
				if (this.sortOrder) searchWhere.order = this.sortOrder
				this.$http.get(`qichechuzu/${this.centerType?'page':'list'}`, {params: Object.assign(params, searchWhere)}).then(res => {
					if (res.data.code == 0) {
						this.dataList = res.data.data.list;
						this.total = Number(res.data.data.total);
						this.pageSize = Number(res.data.data.pageSize);
						this.totalPage = res.data.data.totalPage;
						if(this.pageSizes.length==0){
							this.pageSizes = [this.pageSize, this.pageSize*2, this.pageSize*3, this.pageSize*5];
						}
					}
				});
			},
			sortClick(type){
				if(this.sortType==type){
					if(this.sortOrder == 'desc'){
						this.sortOrder = 'asc'
					}else{
						this.sortOrder = 'desc'
					}
				}else{
					this.sortType = type
					this.sortOrder = 'desc'
				}
				this.getList(1, '全部')
			},
			curChange(page) {
				this.getList(page,this.curFenlei);
			},
			prevClick(page) {
				this.getList(page,this.curFenlei);
			},
			sizeChange(size){
				this.pageSize = size
				this.getList(1,this.curFenlei);
			},
			nextClick(page) {
				this.getList(page,this.curFenlei);
			},
			imgPreView(url){
				this.previewImg = url
				this.previewVisible = true
			},
			toDetail(item) {
				let params = {
					id: item.id
				}
				if(this.centerType){
					params.centerType = 1
				}
				this.$router.push({path: '/index/qichechuzuDetail', query: params});
			},
			btnAuth(tableName,key){
				if(this.centerType){
					return this.isBackAuth(tableName,key)
				}else{
					return this.isAuth(tableName,key)
				}
			},
			getImageUrl(url) {
				if (!url) {
					return '';
				}
				
				// 处理特定的图片路径问题
				if (url.includes('1747643237425.jpg')) {
					return 'http://localhost:8080/springboot0aqexa96/upload/1747643237425.jpg';
				} else if (url.includes('/upload/') && !url.startsWith('http')) {
					return 'http://localhost:8080/springboot0aqexa96' + url;
				} else if (url.startsWith('http')) {
					return url;
				} else if (url.startsWith('/')) {
					return this.baseUrl + url;
				} else {
					return this.baseUrl + url;
				}
			},
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
		
		&:before {
			content: "";
			position: absolute;
			left: 0;
			top: 0;
			bottom: 0;
			width: 6px;
			background: linear-gradient(to bottom, #1B3E90, #25489C);
		}
		
		.breadcrumb-container {
			padding: 20px 0;
			
			/deep/ .el-breadcrumb {
				font-size: 18px;
				
				.el-breadcrumb__item {
					.el-breadcrumb__inner {
						color: #444;
						font-weight: 500;
						transition: all 0.3s;
						
						&:hover {
							color: #1B3E90;
							transform: scale(1.05);
						}
						
						a {
							font-weight: 500;
							color: #444;
							transition: all 0.3s;
							
							&:hover {
								color: #1B3E90;
							}
						}
					}
					
					&:last-child .el-breadcrumb__inner {
						color: #1B3E90;
						font-weight: 700;
						font-size: 19px;
					}
				}
				
				.el-breadcrumb__separator {
					color: #666;
					margin: 0 12px;
					font-size: 20px;
					font-weight: bold;
				}
			}
		}
	}
	
	.list-preview {
		padding: 0 16%;
		margin: 10px auto;
		background: none;
		display: flex;
		width: 100%;
		position: relative;
		flex-wrap: wrap;
		.category-3 {
			padding: 10px ;
			background: #fff;
			display: flex;
			width: 100%;
			height: auto;
			order: 3;
			.item {
				cursor: pointer;
				border: 3px solid rgb(35, 70, 154);
				border-radius: 0 0 30px 0;
				padding: 5px 20px;
				margin: 0 10px 0 0;
				color: #23469A;
				background: linear-gradient(180.00deg, rgb(145, 177, 255),rgb(222, 229, 253) 98.473%);
				display: flex;
				align-items: center;
				img {
					border-radius: 5px;
					margin: 0 5px 0 0;
					object-fit: cover;
					display: block;
					width: 30px;
					height: 30px;
				}
				.text {
					color: inherit;
					font-size: 14px;
				}
			}
			.item:hover {
				border: 3px solid rgb(232, 214, 176);
				color: #AD8B44;
				background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
			}
			.item.active {
				border: 3px solid rgb(232, 214, 176);
				color: #AD8B44;
				background: linear-gradient(180.00deg, rgb(242, 229, 195),rgb(253, 245, 222) 98.473%);
			}
		}
		.list-form-pv {
			padding: 10px;
			background: none;
			display: flex;
			width: 100%;
			align-items: center;
			flex-wrap: wrap;
			height: auto;
			order: 1;
			.list-item {
				margin: 0 10px 10px;
				/deep/.el-form-item__content {
					display: flex;
					width: 100%;
				}
				.lable {
					padding: 0;
					color: #25489C;
					white-space: nowrap;
					flex: 1;
					display: inline-block;
					width: auto;
					line-height: 42px;
				}
				.el-input {
					width: auto;
				}
				.datetimerange {
					border: 1px solid #25489C;
					border-radius: 0;
					padding: 3px 10px;
					outline: none;
					background: #fff;
					width: 100%;
					justify-content: center;
				}
				.el-input /deep/ .el-input__inner {
					border: 1px solid #25489C;
					border-radius: 0;
					padding: 0 10px;
					margin: 0;
					outline: none;
					color: #25489C;
					background: #fff;
					width: 100%;
					font-size: 14px;
					line-height: 42px;
					height: 42px;
				}
				.el-select {
					width: 100%;
				}
				.el-select /deep/ .el-input__inner {
				}
				.el-date-editor {
					width: auto;
				}
				.el-date-editor /deep/ .el-input__inner {
					border: 1px solid #25489C;
					border-radius: 0;
					padding: 0 30px;
					margin: 0;
					outline: none;
					color: #25489C;
					background: #fff;
					width: 100%;
					font-size: 14px;
					line-height: 42px;
					height: 42px;
				}
			}
			.list-search-btn {
				cursor: pointer;
				border: 0;
				border-radius: 0;
				padding: 0px 15px;
				margin: 0 10px 10px 0;
				outline: none;
				color: #fff;
				background: #5D62DE;
				width: auto;
				font-size: 14px;
				line-height: 42px;
				height: 42px;
				i {
					margin: 0 10px 0 0;
					color: #fff;
					font-size: 14px;
				}
			}
			.list-add-btn {
				cursor: pointer;
				border: 0;
				border-radius: 0;
				padding: 0px 15px;
				margin: 0 10px 10px 0;
				outline: none;
				color: #AD8B44;
				background: #E8D6B0;
				width: auto;
				font-size: 14px;
				line-height: 42px;
				height: 42px;
				i {
					margin: 0 10px 0 0;
					color: #fff;
					font-size: 14px;
				}
			}
		}
		.select2 {
			padding: 20px 0;
			background: none;
			width: 100%;
			height: auto;
			order: 2;
			.select2-list {
				padding: 10px 0;
				margin: 4px 0;
				background: none;
				display: flex;
				width: 100%;
				border-color: #25489C;
				border-width: 0 0 2px;
				position: relative;
				border-style: solid;
				height: auto;
				.label {
					padding: 0 10px;
					color: #25489C;
					white-space: nowrap;
					display: inline-block;
					font-size: 14px;
					line-height: 32px;
				}
				.item-body {
					display: flex;
					width: 100%;
					height: auto;
					.item {
						cursor: pointer;
						border-radius: 0;
						padding: 0 20px;
						margin: 0 2px;
						color: #25489C;
						background: none;
						font-size: 14px;
						line-height: 32px;
					}
					.item:hover {
						color: #fff;
						background: #25489C;
					}
					.item.active {
						color: #fff;
						background: #25489C;
					}
				}
			}
		}
		.sort_view {
			padding: 8px 15px;
			margin: 0 0 12px;
			background: #f9f9f9;
			width: 100%;
			order: 4;
			display: flex;
			border-radius: 8px;
			box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
			align-items: center;
			flex-wrap: wrap;
			
			&::before {
				content: "排序:";
				color: #666;
				font-size: 13px;
				font-weight: 500;
				margin-right: 10px;
			}
			
			.click-sort-btn, .collect-sort-btn {
				border: 0;
				border-radius: 16px;
				padding: 0 15px;
				margin: 0 8px 0 0;
				background: linear-gradient(to right, #f8f8f8, #eaeaea);
				height: 30px;
				transition: all 0.3s ease;
				box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
				
				.icon {
					margin: 0 3px 0 0;
					color: #25489C;
					font-size: 13px;
					line-height: 30px;
					transition: all 0.3s ease;
				}
				
				.text {
					color: #25489C;
					font-size: 13px;
					font-weight: 500;
					line-height: 30px;
					transition: all 0.3s ease;
				}
				
				&:hover {
					background: linear-gradient(135deg, #f6efd6, #E8D6B0);
					transform: translateY(-2px);
					box-shadow: 0 4px 8px rgba(173, 139, 68, 0.15);
				}
				
				&.is-active, &[class*="active"] {
					background: linear-gradient(135deg, #23469A, #3561c5);
					box-shadow: 0 3px 6px rgba(35, 70, 154, 0.2);
					
					.icon, .text {
						color: #fff;
					}
				}
			}
			
			.collect-sort-btn.is-active, .collect-sort-btn[class*="active"] {
				background: linear-gradient(135deg, #AD8B44, #c9a25d);
				box-shadow: 0 3px 6px rgba(173, 139, 68, 0.2);
			}
		}
		.list {
			margin: 0 0 10px;
			background: none;
			width: 100%;
			order: 5;
			
			.index-pv1 {
				display: grid;
				grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
				gap: 20px;
				padding: 10px;
				
				.animation-box {
					position: relative;
					background: #fff;
					border-radius: 15px;
					overflow: hidden;
					box-shadow: 0 3px 15px rgba(0, 0, 0, 0.05);
					transition: all 0.3s ease;
					cursor: pointer;
					display: flex;
					flex-direction: column;
					border: 1px solid rgba(232, 214, 176, 0.3);
					
					&:hover {
						transform: translateY(-5px);
						box-shadow: 0 8px 25px rgba(35, 70, 154, 0.15);
						border-color: rgba(35, 70, 154, 0.2);
						
						.image {
							transform: scale(1.05);
						}
						
						.name {
							color: #23469A;
						}
					}
					
					.image {
						width: 100%;
						height: 200px;
						object-fit: cover;
						border-radius: 12px;
						transition: transform 0.5s ease;
						margin-bottom: 10px;
					}
					
					.name {
						padding: 0 15px;
						margin: 5px 0;
						color: #333;
						font-size: 16px;
						font-weight: 600;
						line-height: 1.4;
						transition: color 0.3s ease;
						overflow: hidden;
						text-overflow: ellipsis;
						display: -webkit-box;
						-webkit-line-clamp: 2;
						-webkit-box-orient: vertical;
						min-height: 44px;
					}
					
					.info-container {
						padding: 10px 15px;
						display: grid;
						grid-template-columns: repeat(2, 1fr);
						gap: 8px;
						background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(246,246,246,0.5));
						border-radius: 0 0 15px 15px;
						
						.info-item {
							display: flex;
							align-items: center;
							color: #666;
							font-size: 13px;
							
							.icon {
								margin-right: 5px;
								font-size: 14px;
								
								&.time-icon { color: #E0C936; }
								&.publisher-icon { color: #43BAB1; }
								&.collect-icon { color: #AD8B44; }
								&.view-icon { color: #A76850; }
							}
							
							.text {
								color: inherit;
								overflow: hidden;
								text-overflow: ellipsis;
								white-space: nowrap;
							}
						}
					}
					
					.status-tag {
						position: absolute;
						top: 10px;
						right: 10px;
						padding: 4px 12px;
						border-radius: 20px;
						font-size: 12px;
						font-weight: 500;
						background: rgba(255, 255, 255, 0.9);
						box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
						
						&.available {
							color: #23469A;
							background: rgba(35, 70, 154, 0.1);
						}
						
						&.rented {
							color: #AD8B44;
							background: rgba(173, 139, 68, 0.1);
						}
					}
				}
			}
		}
	}
</style>
