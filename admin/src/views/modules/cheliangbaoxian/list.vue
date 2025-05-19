<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">车辆保险搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-tickets" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">保单编号</label>
							<el-input v-model="searchForm.baodanbianhao" size="medium" placeholder="请输入保单编号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"150px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search"></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-truck" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车车牌</label>
							<el-input v-model="searchForm.qichechepai" size="medium" placeholder="请输入汽车车牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"150px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search"></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-grid" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#E6A23C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车类型</label>
							<el-input v-model="searchForm.qicheleixing" size="medium" placeholder="请输入汽车类型" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"150px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search"></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}' class="select">
							<i class="el-icon-finished" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#909399"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">是否支付</label>
							<el-select clearable size="medium" v-model="searchForm.ispay" placeholder="是否支付" :style='{"borderRadius":"4px","border":"none","width":"150px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}'>
								<el-option v-for="(item,index) in isPayOptions" v-bind:key="index" :label="item" :value="item"></el-option>
							</el-select>
						</div>
						<el-button class="search-btn" type="primary" @click="search()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(64, 158, 255, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-search" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							查询
						</el-button>
						<el-button class="reset-btn" @click="resetSearch()" :style='{"border":"1px solid #dcdfe6","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(0, 0, 0, 0.05)","margin":"0 10px 0 0","outline":"none","color":"#606266","borderRadius":"8px","background":"#fff","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-refresh-right" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#606266","height":"36px"}'></i>
							重置
						</el-button>
					</el-row>
					<el-row class="actions" :style='{"padding":"10px 0 0","margin":"10px 0 0","borderTop":"1px dashed #ebeef5","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}'>
						<el-button class="add-btn" v-if="isAuth('cheliangbaoxian','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('cheliangbaoxian','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							删除
						</el-button>
						<el-button class="pay-btn" v-if="isAuth('cheliangbaoxian','支付')" :disabled="dataListSelections.length?false:true" type="warning" @click="payBatch()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(230, 162, 60, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#E6A23C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-wallet" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							批量支付
						</el-button>
					</el-row>
				</el-form>
			</el-card>

			<!-- 列表卡片 -->
			<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
				<div class="table-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<div class="table-title" style="display: flex; align-items: center;">
						<i class="el-icon-tickets" style="font-size: 18px; color: #67C23A; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">车辆保险列表</span>
						<span class="table-count" style="font-size: 13px; color: #909399; margin-left: 8px;">(共 {{totalPage}} 条记录)</span>
					</div>
					<div class="table-actions">
						<el-tooltip content="刷新" placement="top">
							<el-button size="mini" type="text" icon="el-icon-refresh" circle @click="search()" style="font-size: 16px; color: #909399;"></el-button>
						</el-tooltip>
						<el-tooltip content="密度" placement="top">
							<el-dropdown trigger="click" @command="handleSizeChange">
								<el-button size="mini" type="text" icon="el-icon-s-grid" circle style="font-size: 16px; color: #909399;"></el-button>
								<el-dropdown-menu slot="dropdown">
									<el-dropdown-item command="medium">默认</el-dropdown-item>
									<el-dropdown-item command="small">紧凑</el-dropdown-item>
									<el-dropdown-item command="mini">迷你</el-dropdown-item>
								</el-dropdown-menu>
							</el-dropdown>
						</el-tooltip>
					</div>
				</div>
				<el-table
					class="tables"
					:stripe="true"
					:size="tableSize"
					:style="{padding:'0',borderColor:'#ebeef5',borderRadius:'12px',borderWidth:'1px',background:'#fff',width:'100%',fontSize:'12px',borderStyle:'solid',maxWidth:'100vw',tableLayout:'fixed'}"
					:border="false"
					v-if="isAuth('cheliangbaoxian','查看')"
					:data="dataList"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'500', 'fontSize':'12px', 'background-color':'#f5f7fa', 'padding':'4px 0', 'text-align': 'center'}"
					:cell-style="{'color':'#333', 'fontSize':'12px', 'padding':'4px 0', 'text-align': 'center'}"
				>
					<el-table-column type="selection" align="center" width="30" />
					<el-table-column label="序号" type="index" width="30" align="center" :index="indexMethod" />
					<el-table-column prop="baodanbianhao" label="保单编号" align="center" width="60" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.baodanbianhao}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="qichechepai" label="车牌" align="center" width="50" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.qichechepai}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="qicheleixing" label="类型" align="center" width="50" show-overflow-tooltip>
						<template slot-scope="scope">
							<el-tag size="mini" type="info" effect="light">{{scope.row.qicheleixing}}</el-tag>
						</template>
					</el-table-column>
					<el-table-column prop="baoxianmingcheng" label="保险名称" align="center" width="60" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.baoxianmingcheng}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="banlishijian" label="办理时间" align="center" width="60" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.banlishijian}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="daoqishijian" label="到期时间" align="center" width="60" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.daoqishijian}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="baoxianfeiyong" label="费用" align="center" width="50" show-overflow-tooltip>
						<template slot-scope="scope">
							<span>{{scope.row.baoxianfeiyong}}</span>
						</template>
					</el-table-column>
					<el-table-column prop="qichetupian" label="图" align="center" width="32">
						<template slot-scope="scope">
							<div v-if="scope.row.qichetupian" class="image-container">
								<el-image
									:src="scope.row.qichetupian && scope.row.qichetupian.substring(0,4)=='http' ? 
										(scope.row.qichetupian.split(',w').length>1 ? scope.row.qichetupian : scope.row.qichetupian.split(',')[0]) 
										: $base.url+scope.row.qichetupian.split(',')[0]"
									style="width:20px;height:20px;border-radius:2px;object-fit:cover;"
								></el-image>
							</div>
						</template>
					</el-table-column>
					<el-table-column prop="ispay" label="支付" align="center" width="50" show-overflow-tooltip>
						<template slot-scope="scope">
							<el-tag v-if="scope.row.ispay=='已支付'" type="success">已支付</el-tag>
							<el-tag v-else type="danger">未支付</el-tag>
							<el-button v-if="scope.row.ispay!='已支付' && isAuth('cheliangbaoxian','支付') " type="text" size="mini" @click="payHandler(scope.row)">支付</el-button>
						</template>
					</el-table-column>
					<el-table-column label="操作" align="center" width="90">
						<template slot-scope="scope">
							<el-button type="text" icon="el-icon-view" size="mini" v-if="isAuth('cheliangbaoxian','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn">查看</el-button>
							<el-button type="text" icon="el-icon-edit" size="mini" v-if="isAuth('cheliangbaoxian','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn">修改</el-button>
						</template>
					</el-table-column>
				</el-table>
				<!-- 分页部分 -->
				<el-pagination
					@size-change="sizeChangeHandle"
					@current-change="currentChangeHandle"
					:current-page="pageIndex"
					background
					:page-sizes="[10, 20, 50, 100]"
					:page-size="pageSize"
					:layout="layouts.join()"
					:total="totalPage"
					prev-text="< "
					next-text="> "
					:hide-on-single-page="false"
					class="custom-pagination"
					style="padding: 15px 20px; margin: 20px auto; white-space: nowrap; color: #606266; text-align: center; background: #ffffff; border-radius: 8px; width: 98%; font-size: 13px; box-shadow: 0 1px 8px rgba(0, 0, 0, 0.05); border: 1px solid #ebeef5;"
				></el-pagination>
			</el-card>
		</template>
		<!-- 添加/修改页面  将父组件的search方法传递给子组件-->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>
		<!-- 其它弹窗和图表部分保持不变 -->
	</div>
</template>

<script>
	import axios from 'axios';
	import AddOrUpdate from "./add-or-update";
	import {
		Loading
	} from 'element-ui';
	export default {
		data() {
			return {
				indexQueryCondition: '',
				searchForm: {
					key: ""
				},
				form:{},
				isPayOptions: [],
				dataList: [],
				pageIndex: 1,
				pageSize: 10,
				totalPage: 0,
				dataListLoading: false,
				dataListSelections: [],
				showFlag: true,
				addOrUpdateFlag:false,
				layouts: ["total","prev","pager","next","sizes","jumper"],
				previewImg: '',
				previewVisible: false,
			};
		},
		created() {
			this.init();
			this.getDataList();
			this.contentStyleChange();
		},
		mounted() {
		},
		filters: {
			htmlfilter: function (val) {
				return val.replace(/<[^>]*>/g).replace(/undefined/g,'');
			}
		},
		computed: {
			tablename(){
				return this.$storage.get('sessionTable')
			},
		},
		components: {
			AddOrUpdate,
		},
		methods: {
			imgPreView(url){
				this.previewImg = url
				this.previewVisible = true
			},
			contentStyleChange() {
				this.contentPageStyleChange()
			},
			// 分页
			contentPageStyleChange(){
				let arr = []

				// if(this.contents.pageTotal) arr.push('total')
				// if(this.contents.pageSizes) arr.push('sizes')
				// if(this.contents.pagePrevNext){
				//   arr.push('prev')
				//   if(this.contents.pagePager) arr.push('pager')
				//   arr.push('next')
				// }
				// if(this.contents.pageJumper) arr.push('jumper')
				// this.layouts = arr.join()
				// this.contents.pageEachNum = 10
			},
			payHandler(row){
				this.$storage.set('paytable','cheliangbaoxian');
				this.$storage.set('payObject',row);
				this.$router.push('pay');
			},
			// 批量支付
			payBatch(){
				for(let x in this.dataListSelections){
					if(this.dataListSelections[x].ispay=='已支付'){
						this.$message.error('所选订单存在已支付订单')
						return false
					}
				}
				this.$confirm('是否支付所选订单？').then(_ => {
					let arr = JSON.parse(JSON.stringify(this.dataListSelections))
					for(let i in arr){
						arr[i].ispay = '已支付'
						this.$http({
							url: 'cheliangbaoxian/update',
							method: "post",
							data: arr[i]
						}).then(res=>{
							
						})
					}
					this.$message({
						message: "支付成功",
						type: "success",
						duration: 1500,
						onClose: () => {
							this.getDataList()
						}
					});
				}).catch(_ => {});
			},





			init () {
				this.isPayOptions = "已支付,未支付".split(',')
			},
			search() {
				this.pageIndex = 1;
				this.getDataList();
			},

			// 获取数据列表
			getDataList() {
				this.dataListLoading = true;
				let params = {
					page: this.pageIndex,
					limit: this.pageSize,
					sort: 'id',
					order: 'desc',
				}
 				if(this.searchForm.baodanbianhao!='' && this.searchForm.baodanbianhao!=undefined){
					params['baodanbianhao'] = '%' + this.searchForm.baodanbianhao + '%'
				}
 				if(this.searchForm.qichechepai!='' && this.searchForm.qichechepai!=undefined){
					params['qichechepai'] = '%' + this.searchForm.qichechepai + '%'
				}
 				if(this.searchForm.qicheleixing!='' && this.searchForm.qicheleixing!=undefined){
					params['qicheleixing'] = '%' + this.searchForm.qicheleixing + '%'
				}
				if(this.searchForm.baoxianleixing!='' && this.searchForm.baoxianleixing!=undefined){
					params['baoxianleixing'] = this.searchForm.baoxianleixing
				}
				if(this.searchForm.ispay!='' && this.searchForm.ispay!=undefined){
					params['ispay'] = this.searchForm.ispay
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "cheliangbaoxian/page",
					method: "get",
					params: params
				}).then(({ data }) => {
					if (data && data.code === 0) {
						this.dataList = data.data.list;
						this.totalPage = data.data.total;
					} else {
						this.dataList = [];
						this.totalPage = 0;
					}
					this.dataListLoading = false;
				});
			},
			// 每页数
			sizeChangeHandle(val) {
				this.pageSize = val;
				this.pageIndex = 1;
				this.getDataList();
			},
			// 当前页
			currentChangeHandle(val) {
				this.pageIndex = val;
				this.getDataList();
			},
			// 多选
			selectionChangeHandler(val) {
				this.dataListSelections = val;
			},
			// 添加/修改
			addOrUpdateHandler(id,type) {
				this.showFlag = false;
				this.addOrUpdateFlag = true;
				this.crossAddOrUpdateFlag = false;
				if(type!='info'&&type!='msg'){
					type = 'else';
				}
				this.$nextTick(() => {
					this.$refs.addOrUpdate.init(id,type);
				});
			},
			// 下载
			download(file){
				let arr = file.replace(new RegExp('upload/', "g"), "")
				axios.get(this.$base.url + 'file/download?fileName=' + arr, {
					headers: {
						token: this.$storage.get('Token')
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
					axios.get((location.href.split(this.$base.name).length>1 ? location.href.split(this.$base.name)[0] :'') + this.$base.name + '/file/download?fileName=' + arr, {
						headers: {
							token: this.$storage.get('Token')
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
			// 删除
			async deleteHandler(id ) {
				var ids = id? [Number(id)]: this.dataListSelections.map(item => {
					return Number(item.id);
				});
				await this.$confirm(`确定进行[${id ? "删除" : "批量删除"}]操作?`, "提示", {
					confirmButtonText: "确定",
					cancelButtonText: "取消",
					type: "warning"
				}).then(async () => {
					await this.$http({
						url: "cheliangbaoxian/delete",
						method: "post",
						data: ids
					}).then(async ({ data }) => {
						if (data && data.code === 0) {
							this.$message({
								message: "操作成功",
								type: "success",
								duration: 1500,
								onClose: () => {
									this.search();
								}
							});
			
						} else {
							this.$message.error(data.msg);
						}
					});
				});
			},


		}

	};
</script>
<style lang="css" scoped>
	
	.center-form-pv {
		.el-date-editor.el-input {
			width: auto;
		}
	}
	
	.el-input {
		width: auto;
	}
	
	.center-form-pv .el-input {
		width: auto;
	}
	.center-form-pv .el-input /deep/ .el-input__inner {
		border: 1px solid #ddd;
		border-radius: 4px;
		padding: 0 12px;
		color: #666;
		width: 150px;
		font-size: 15px;
		height: 34px;
	}
	.center-form-pv .el-select {
		width: auto;
	}
	.center-form-pv .el-select /deep/ .el-input__inner {
		border: 1px solid #ddd;
		border-radius: 4px;
		padding: 0 10px;
		color: #666;
		width: 150px;
		font-size: 15px;
		height: 34px;
	}
	.center-form-pv .el-date-editor {
		width: auto;
	}
	
	.center-form-pv .el-date-editor /deep/ .el-input__inner {
		border: 1px solid #ddd;
		border-radius: 0px;
		padding: 0 10px 0 30px;
		color: #666;
		width: 150px;
		font-size: 15px;
		height: 34px;
	}
	
	.center-form-pv .search {
		border: 0;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 10px 0 5px;
		color: #fff;
		background: #1fc3cb;
		width: auto;
		font-size: 16px;
		height: 34px;
	}
	
	.center-form-pv .search:hover {
		opacity: 0.8;
	}
	
	.center-form-pv .actions .add {
		border: 0px solid #ddd;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 10px;
		margin: 4px;
		color: #fff;
		background: #1fc3cb;
		width: auto;
		font-size: inherit;
		height: 34px;
	}
	
	.center-form-pv .actions .add:hover {
		opacity: 0.8;
	}
	
	.center-form-pv .actions .del {
		border: 0px solid #ddd;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 10px;
		margin: 4px;
		color: #fff;
		background: #be5353;
		width: auto;
		font-size: inherit;
		height: 34px;
	}
	
	.center-form-pv .actions .del:hover {
		opacity: 0.8;
	}
	
	.center-form-pv .actions .statis {
		border: 0px solid #ddd;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 10px;
		margin: 4px;
		color: #fff;
		background: #3fc182;
		width: auto;
		font-size: inherit;
		height: 34px;
	}
	
	.center-form-pv .actions .statis:hover {
		opacity: 0.8;
	}
	
	.center-form-pv .actions .btn18 {
		border: 1px solid #939393;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 10px;
		margin: 4px;
		color: #939393;
		background: #fff;
		width: auto;
		font-size: inherit;
		height: 34px;
	}
	
	.center-form-pv .actions .btn18:hover {
		opacity: 0.8;
	}
	
	.el-table /deep/ .el-table__header-wrapper thead {
		color: #999;
		background: #f7f7f7;
		font-weight: 500;
		width: 100%;
	}
	
	.el-table /deep/ .el-table__header-wrapper thead tr {
		background: none;
	}
	
	.el-table /deep/ .el-table__header-wrapper thead tr th {
		padding: 8px 0;
		background: none;
		border-color: #ddd;
		border-width: 0 0px 0px 0;
		border-style: solid;
		text-align: left;
	}

	.el-table /deep/ .el-table__header-wrapper thead tr th .cell {
		padding: 0 0 0 5px;
		word-wrap: normal;
		color: #333;
		white-space: normal;
		font-weight: bold;
		display: flex;
		vertical-align: middle;
		font-size: 14px;
		line-height: 24px;
		text-overflow: ellipsis;
		word-break: break-all;
		width: 100%;
		align-items: center;
		position: relative;
		min-width: 110px;
	}

	
	.el-table /deep/ .el-table__body-wrapper tbody {
		width: 100%;
	}

	.el-table /deep/ .el-table__body-wrapper tbody tr {
		background: #fff;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td {
		padding: 4px 0;
		color: #333;
		background: #fff;
		font-size: inherit;
		border-color: #eee;
		border-width: 0 0px 1px 0;
		border-style: solid;
		text-align: left;
	}
	
		
	.el-table /deep/ .el-table__body-wrapper tbody tr:hover td {
		padding: 4px 0;
		color: #333;
		background: #fcfcfc;
		border-color: #eee;
		border-width: 0 0px 1px 0;
		border-style: solid;
		text-align: left;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td {
		padding: 4px 0;
		color: #333;
		background: #fff;
		font-size: inherit;
		border-color: #eee;
		border-width: 0 0px 1px 0;
		border-style: solid;
		text-align: left;
	}

	.el-table /deep/ .el-table__body-wrapper tbody tr td .cell {
		padding: 0 0 0 5px;
		overflow: hidden;
		word-break: break-all;
		white-space: normal;
		font-size: inherit;
		line-height: 24px;
		text-overflow: ellipsis;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .view {
		border: 0px solid #157ed2;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 5px;
		margin: 0 5px 5px 0;
		color: #fff;
		background: #1fc3cb;
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .view:hover {
		opacity: 0.8;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .add {
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .add:hover {
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .edit {
		border: 0px solid #157ed2;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 5px;
		margin: 0 5px 5px 0;
		color: #fff;
		background: #24BF74;
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .edit:hover {
		opacity: 0.8;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .del {
		border: 0px solid #157ed2;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 5px;
		margin: 0 5px 5px 0;
		color: #fff;
		background: #be5353;
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .del:hover {
		opacity: 0.8;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .btn8 {
		border: 1px solid #939393;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 5px;
		margin: 4px;
		color: #939393;
		background: #fff;
		width: auto;
		font-size: inherit;
		height: 34px;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .btn8:hover {
		opacity: 0.8;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__total {
		margin: 0 10px 0 0;
		color: #666;
		font-weight: 400;
		display: inline-block;
		vertical-align: top;
		font-size: inherit;
		line-height: 28px;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .btn-prev {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: none;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: auto;
		min-width: 35px;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .btn-next {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: none;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: auto;
		min-width: 35px;
		height: 28px;
	}

	.main-content .el-pagination /deep/ .btn-prev:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: none;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: auto;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .btn-next:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: none;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: auto;
		height: 28px;
	}

	.main-content .el-pagination /deep/ .el-pager {
		padding: 0;
		margin: 0;
		display: inline-block;
		vertical-align: top;
	}

	.main-content .el-pagination /deep/ .el-pager .number {
		cursor: pointer;
		border-radius: 2px;
		padding: 0 10px;
		margin: 0;
		color: #666;
		background: none;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: 28px;
		text-align: center;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pager .number:hover {
		cursor: pointer;
		border-radius: 100%;
		padding: 0 10px;
		margin: 0;
		color: #fff;
		background: #1fc3cb;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: 28px;
		text-align: center;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pager .number.active {
		cursor: default;
		border-radius: 100%;
		padding: 0 10px;
		margin: 0;
		color: #fff;
		background: #1fc3cb;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: 28px;
		text-align: center;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes {
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: 28px;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input {
		margin: 0 5px;
		width: 100px;
		position: relative;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 25px 0 8px;
		color: #606266;
		display: inline-block;
		font-size: 16px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input span.el-input__suffix {
		top: 0;
		position: absolute;
		right: 0;
		height: 100%;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__sizes .el-input .el-input__suffix .el-select__caret {
		cursor: pointer;
		color: #C0C4CC;
		width: 25px;
		font-size: 14px;
		line-height: 28px;
		text-align: center;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__jump {
		margin: 0 0 0 24px;
		color: #606266;
		display: inline-block;
		vertical-align: top;
		font-size: 16px;
		line-height: 28px;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__jump .el-input {
		border-radius: 3px;
		padding: 0 2px;
		margin: 0 2px;
		display: inline-block;
		width: 50px;
		font-size: 14px;
		line-height: 18px;
		position: relative;
		text-align: center;
		height: 28px;
	}
	
	.main-content .el-pagination /deep/ .el-pagination__jump .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 3px;
		color: #606266;
		display: inline-block;
		font-size: 16px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
	
	.one .list1-view {
		border: 0;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 15px;
		margin: 0 5px 5px 0;
		outline: none;
		color: #fff;
		background: #157ed2;
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.one .list1-view:hover {
		opacity: 0.8;
	}
	
	.one .list1-edit {
		border: 0;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 15px;
		margin: 0 5px 5px 0;
		outline: none;
		color: #fff;
		background: #409eff;
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.one .list1-edit:hover {
		opacity: 0.8;
	}
	
	.one .list1-del {
		border: 0;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 15px;
		margin: 0 5px 5px 0;
		outline: none;
		color: #fff;
		background: rgba(255, 0, 0, 1);
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.one .list1-del:hover {
		opacity: 0.8;
	}
	
	.one .list1-btn8 {
		border: 0;
		cursor: pointer;
		border-radius: 4px;
		padding: 0 24px;
		margin: 0 5px 5px 0;
		outline: none;
		color: #fff;
		background: rgba(255, 128, 0, 1);
		width: auto;
		font-size: 14px;
		height: 32px;
	}
	
	.one .list1-btn8:hover {
		opacity: 0.8;
	}
	
	.main-content .el-table .el-switch {
		display: inline-flex;
		vertical-align: middle;
		line-height: 30px;
		position: relative;
		align-items: center;
		height: 30px;
	}
	.main-content .el-table .el-switch /deep/ .el-switch__label--left {
		cursor: pointer;
		margin: 0 10px 0 0;
		color: #333;
		font-weight: 500;
		display: inline-block;
		vertical-align: middle;
		font-size: 16px;
		transition: .2s;
		height: 30px;
	}
	.main-content .el-table .el-switch /deep/ .el-switch__label--right {
		cursor: pointer;
		margin: 0 0 0 10px;
		color: #333;
		font-weight: 500;
		display: inline-block;
		vertical-align: middle;
		font-size: 16px;
		transition: .2s;
		height: 30px;
	}
	.main-content .el-table .el-switch /deep/ .el-switch__core {
		border: 1px solid #75c0d6;
		cursor: pointer;
		border-radius: 15px;
		margin: 0;
		background: #75c0d6;
		display: inline-block;
		width: 42px;
		box-sizing: border-box;
		transition: border-color .3s,background-color .3s;
		height: 20px;
	}
	.main-content .el-table .el-switch /deep/ .el-switch__core::after {
		border-radius: 100%;
		top: 1px;
		left: 1px;
		background: #fff;
		width: 16px;
		position: absolute;
		transition: all .3s;
		height: 16px;
	}
	.main-content .el-table .el-switch.is-checked /deep/ .el-switch__core::after {
		margin: 0 0 0 -18px;
		left: 100%;
	}
	
	.main-content .el-table .el-rate /deep/ .el-rate__item {
		cursor: pointer;
		display: inline-block;
		vertical-align: middle;
		font-size: 0;
		position: relative;
	}
	.main-content .el-table .el-rate /deep/ .el-rate__item .el-rate__icon {
		margin: 0 3px;
		display: inline-block;
		font-size: 18px;
		position: relative;
		transition: .3s;
	}

</style>
