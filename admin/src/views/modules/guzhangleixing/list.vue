<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">故障类型搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-warning" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#E6A23C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">故障类型</label>
							<el-input v-model="searchForm.guzhangleixing" size="medium" placeholder="请输入故障类型" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#E6A23C","margin":"0 5px 0 0"}'></el-input>
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
						<el-button class="add-btn" v-if="isAuth('guzhangleixing','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('guzhangleixing','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							删除
						</el-button>
					</el-row>
				</el-form>
			</el-card>
			
			<!-- 列表卡片 -->
			<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
				<div class="table-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<div class="table-title" style="display: flex; align-items: center;">
						<i class="el-icon-warning" style="font-size: 18px; color: #E6A23C; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">故障类型列表</span>
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
				
				<!-- 表格部分 -->
				<el-table 
					class="tables"
					:stripe='true'
					:size="tableSize"
					:style='{"padding":"0","borderColor":"#ebeef5","borderRadius":"12px","borderWidth":"1px","background":"#fff","width":"100%","fontSize":"14px","borderStyle":"solid"}' 
					:border='false'
					v-if="isAuth('guzhangleixing','查看')"
					:data="dataList"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'500', 'fontSize':'14px', 'background-color':'#f5f7fa', 'padding':'12px 0', 'text-align': 'center'}"
					:cell-style="{'color':'#333', 'fontSize':'14px', 'padding':'10px 0', 'text-align': 'center'}">
					<el-table-column :resizable='true' type="selection" align="center" width="50"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="60" align="center" :index="indexMethod"></el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="guzhangleixing"
						label="故障类型"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="type-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-warning" style="color: #E6A23C; margin-right: 5px;"></i>
								<span>{{scope.row.guzhangleixing}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						width="160" 
						label="操作"
						align="center">
						<template slot-scope="scope">
							<div class="operation-buttons" style="display: flex; justify-content: center; flex-wrap: wrap;">
								<el-button type="text" icon="el-icon-edit" size="mini" v-if="isAuth('guzhangleixing','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn" style="margin: 2px 5px; color: #E6A23C;">修改</el-button>
								<el-button type="text" icon="el-icon-delete" size="mini" v-if="isAuth('guzhangleixing','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn" style="margin: 2px 5px; color: #F56C6C;">删除</el-button>
							</div>
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
	</div>
</template>

<script>
	import axios from 'axios';
	import AddOrUpdate from "./add-or-update";
	import { Loading } from 'element-ui';
	
	export default {
		data() {
			return {
				indexQueryCondition: '',
				searchForm: {
					key: ""
				},
				form:{},
				dataList: [],
				pageIndex: 1,
				pageSize: 10,
				totalPage: 0,
				dataListLoading: false,
				dataListSelections: [],
				showFlag: true,
				addOrUpdateFlag:false,
				layouts: ["total","prev","pager","next","sizes","jumper"],
				tableSize: 'medium',
			};
		},
		components: {
			AddOrUpdate,
		},
		mounted() {
			this.init();
			this.getDataList();
		},
		methods: {
			// 添加重置搜索方法
			resetSearch() {
				this.searchForm = {
					key: ""
				};
				this.getDataList();
			},
			
			// 表格密度调整
			handleSizeChange(command) {
				this.tableSize = command;
			},
			
			// 索引方法
			indexMethod(index) {
				return (this.pageIndex - 1) * this.pageSize + index + 1;
			},
			
			init () {
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
				if(this.searchForm.guzhangleixing!='' && this.searchForm.guzhangleixing!=undefined){
					params['guzhangleixing'] = '%' + this.searchForm.guzhangleixing + '%'
				}
				this.$http({
					url: "guzhangleixing/page",
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
			addOrUpdateHandler(id) {
				this.showFlag = false;
				this.addOrUpdateFlag = true;
				this.$nextTick(() => {
					this.$refs.addOrUpdate.init(id);
				});
			},
			// 删除
			deleteHandler(id) {
				var ids = id ? [Number(id)] : this.dataListSelections.map(item => {
					return Number(item.id);
				});
				this.$confirm(`确定进行[${id ? "删除" : "批量删除"}]操作?`, "提示", {
					confirmButtonText: "确定",
					cancelButtonText: "取消",
					type: "warning"
				}).then(() => {
					this.$http({
						url: "guzhangleixing/delete",
						method: "post",
						data: ids
					}).then(({ data }) => {
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

<style>
.custom-pagination .btn-prev,
.custom-pagination .btn-next,
.custom-pagination .el-pager li {
  background-color: #f5f7fa;
  color: #606266;
  border-radius: 4px;
  margin: 0 3px;
}

.custom-pagination .el-pager li.active {
  background-color: #409EFF;
  color: #fff;
}

.custom-pagination .btn-prev:hover,
.custom-pagination .btn-next:hover,
.custom-pagination .el-pager li:hover:not(.active) {
  color: #409EFF;
}

.operation-buttons .el-button--text {
  padding: 4px 8px;
  border-radius: 4px;
  transition: all 0.3s;
}

.operation-buttons .el-button--text:hover {
  background-color: rgba(64, 158, 255, 0.1);
}

.edit-btn:hover {
  color: #eebe77 !important;
}

.delete-btn:hover {
  color: #f78989 !important;
}

.type-info {
  transition: all 0.3s ease;
}
</style>
