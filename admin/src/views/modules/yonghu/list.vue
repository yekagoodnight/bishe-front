<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">用户搜索</span>
					</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-user" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">用户账号</label>
							<el-input v-model="searchForm.yonghuzhanghao" size="medium" placeholder="请输入用户账号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
					</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-notebook-1" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">用户姓名</label>
							<el-input v-model="searchForm.yonghuxingming" size="medium" placeholder="请输入用户姓名" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#67C23A","margin":"0 5px 0 0"}'></el-input>
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
						<el-button class="add-btn" v-if="isAuth('yonghu','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
						添加
					</el-button>
						<el-button class="del-btn" v-if="isAuth('yonghu','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
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
						<i class="el-icon-tickets" style="font-size: 18px; color: #67C23A; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">用户列表</span>
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
					v-if="isAuth('yonghu','查看')"
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
						prop="yonghuzhanghao"
						label="用户账号"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="account-info" style="justify-content: center;">
								<span class="account-text">{{scope.row.yonghuzhanghao}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="yonghuxingming"
						label="用户姓名"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="name-info" style="justify-content: center;">
								<span class="name-text">{{scope.row.yonghuxingming}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="xingbie"
						label="性别"
						align="center"
						width="80">
						<template slot-scope="scope">
							<el-tag size="medium" :type="scope.row.xingbie === '男' ? 'primary' : 'success'" effect="light">
								<i :class="scope.row.xingbie === '男' ? 'el-icon-male' : 'el-icon-female'" style="margin-right: 5px;"></i>
							{{scope.row.xingbie}}
							</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="shouji"
						label="手机"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="phone-info" style="justify-content: center;">
								<i class="el-icon-mobile-phone" style="color: #909399; margin-right: 5px;"></i>
								<span>{{scope.row.shouji}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="shenfenzheng"
						label="身份证"
						align="center"
						min-width="180">
						<template slot-scope="scope">
							<div class="id-info" style="justify-content: center;">
								<i class="el-icon-document" style="color: #909399; margin-right: 5px;"></i>
								<span>{{scope.row.shenfenzheng}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						prop="touxiang" 
						width="100" 
						label="头像" 
						align="center">
						<template slot-scope="scope">
							<div v-if="scope.row.touxiang" class="avatar-container">
								<el-image
									:src="getAvatarUrl(scope.row.touxiang)"
									:preview-src-list="[getAvatarUrl(scope.row.touxiang)]"
									style="width: 60px; height: 60px; border-radius: 50%; object-fit: cover; cursor: pointer; border: 3px solid #fff; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);"
								></el-image>
							</div>
							<div v-else class="no-avatar">
								<i class="el-icon-user" style="font-size: 24px; color: #909399;"></i>
								<span style="font-size: 12px; color: #909399; margin-top: 5px;">无头像</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						width="180" 
						label="操作" 
						align="center">
						<template slot-scope="scope">
							<div class="operation-buttons">
								<el-button type="text" icon="el-icon-view" size="small" v-if="isAuth('yonghu','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn">查看</el-button>
								<el-button type="text" icon="el-icon-edit" size="small" v-if="isAuth('yonghu','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn">修改</el-button>
								<el-button type="text" icon="el-icon-delete" size="small" v-if="isAuth('yonghu','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn">删除</el-button>
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
		
		<!-- 添加/修改页面 -->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>

        <!-- 图片预览弹窗 -->
		<el-dialog title="图片预览" :visible.sync="previewVisible" width="50%" :before-close="handleDialogClose">
			<img :src="previewImg" alt="" style="width: 100%;">
		</el-dialog>
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
				tableSize: 'medium',
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
			handleDialogClose(done) {
				this.previewVisible = false
				done()
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

			// 自定义序号方法
			indexMethod(index) {
				return (this.pageIndex - 1) * this.pageSize + index + 1;
			},

			init () {
			},
			search() {
				this.pageIndex = 1;
				this.getDataList();
			},
			
			// 重置搜索表单
			resetSearch() {
				this.searchForm = {
					key: "",
					yonghuzhanghao: "",
					yonghuxingming: "",
					xingbie: ""
				};
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
 				if(this.searchForm.yonghuzhanghao!='' && this.searchForm.yonghuzhanghao!=undefined){
					params['yonghuzhanghao'] = '%' + this.searchForm.yonghuzhanghao + '%'
				}
 				if(this.searchForm.yonghuxingming!='' && this.searchForm.yonghuxingming!=undefined){
					params['yonghuxingming'] = '%' + this.searchForm.yonghuxingming + '%'
				}
				if(this.searchForm.xingbie!='' && this.searchForm.xingbie!=undefined){
					params['xingbie'] = this.searchForm.xingbie
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "yonghu/page",
					method: "get",
					params: params
				}).then(({ data }) => {
					if (data && data.code === 0) {
						this.dataList = data.data.list;
						this.totalPage = data.data.total;
					} else {
						this.$message.error(data.msg);
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
						url: "yonghu/delete",
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
			handleSizeChange(command) {
				this.tableSize = command;
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
<style>
.main-content {
  min-height: calc(100vh - 160px);
}

/* 分页容器样式 */
.custom-pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.2s ease;
  position: relative;
}

.custom-pagination::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 2px;
  background: linear-gradient(to right, #f0f2f5, #ebeef5, #f0f2f5);
}

/* 分页内部元素样式 */
.custom-pagination .el-pagination__total {
  color: #606266;
  font-weight: 500;
}

.custom-pagination .el-pager li {
		border-radius: 4px;
  min-width: 32px;
  height: 32px;
  line-height: 32px;
  font-weight: 500;
}

.custom-pagination .el-pager li.active {
  background-color: #409EFF;
  color: white;
  font-weight: 500;
}

.custom-pagination .el-pager li:hover:not(.active) {
  color: #409EFF;
}

.custom-pagination .btn-prev,
.custom-pagination .btn-next {
  background-color: transparent;
}

.custom-pagination .btn-prev:hover:not(:disabled),
.custom-pagination .btn-next:hover:not(:disabled) {
  color: #409EFF;
}

.custom-pagination .el-pagination__sizes .el-input,
.custom-pagination .el-pagination__jump .el-input {
  margin: 0 5px;
  width: 110px;
}

.custom-pagination .el-input__inner {
  height: 28px;
  line-height: 28px;
  background: #f5f7fa;
  border: 1px solid #e4e7ed;
		border-radius: 4px;
  transition: all 0.3s;
  color: #606266;
  font-size: 13px;
}

.custom-pagination .el-input__inner:hover {
  border-color: #c0c4cc;
}

.custom-pagination .el-input__inner:focus {
  border-color: #409EFF;
  background-color: #fff;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
}

.custom-pagination .el-select-dropdown__item {
  padding: 0 15px;
  font-size: 13px;
}

.custom-pagination .el-pagination__jump {
  margin-left: 15px;
  font-weight: normal;
  color: #606266;
}

.custom-pagination .el-pagination__total {
  margin-right: 15px;
  font-weight: normal;
  color: #606266;
}

/* 统一分页按钮样式 */
.custom-pagination .btn-prev,
.custom-pagination .btn-next,
.custom-pagination .el-pager li {
  background-color: #f4f4f5;
  color: #606266;
  border: none;
  font-weight: normal;
  padding: 0;
  margin: 0 3px;
  min-width: 30px;
  height: 30px;
  line-height: 30px;
  border-radius: 3px;
  transition: all 0.2s;
}

.custom-pagination .el-pager li.active {
  background-color: #409EFF;
  color: #fff;
  font-weight: 500;
}

.custom-pagination .btn-prev:hover,
.custom-pagination .btn-next:hover,
.custom-pagination .el-pager li:hover:not(.active) {
  background-color: #e9e9eb;
  color: #409EFF;
}

/* 表格相关样式 */
.tables {
  border: none !important;
  margin-bottom: 10px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.05);
}

/* 表格相关样式 */
.tables {
  border: none !important;
  margin-bottom: 10px;
  box-shadow: 0 1px 5px rgba(0, 0, 0, 0.05);
}

.tables .el-table__body-wrapper {
  padding: 5px 0;
}

.tables .el-table__header-wrapper th {
  background-color: #f5f7fa;
		color: #333;
  font-weight: 500;
		font-size: 14px;
  padding: 12px 0;
  letter-spacing: 0.5px;
}

.tables .el-table__row {
  cursor: pointer;
  transition: all 0.3s;
		position: relative;
  z-index: 1;
  margin: 5px 0;
}

.tables .el-table__body tr.hover-row > td.el-table__cell {
  background-color: #f0f5ff !important;
}
    
.tables .el-table__row:hover {
  background-color: #f0f5ff !important;
  transform: translateY(-2px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
  z-index: 2;
}

.tables .el-table__row:hover td {
  background-color: transparent !important;
}
    
.tables .el-table__row td {
  padding: 10px 0;
}

/* 表格标题和操作区域 */
.table-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.table-actions .el-button {
  transition: all 0.3s;
}

.table-actions .el-button:hover {
  transform: rotate(180deg);
  color: #409EFF !important;
}

/* 表格边框 */
.el-table th.is-leaf, .el-table td {
  border-bottom: 1px solid #ebeef5;
}

.el-table--striped .el-table__body tr.el-table__row--striped td {
  background: #fafafa;
}

/* 文字显示优化 */
.account-text, .name-text {
  font-weight: 500;
  color: #303133;
  text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
  letter-spacing: 0.5px;
}

.phone-info span, .id-info span {
  color: #606266;
  font-family: "Roboto", sans-serif;
  letter-spacing: 0.5px;
}

.phone-info i, .id-info i {
  position: relative;
  top: 1px;
}

/* 表格滚动条美化 */
.el-table__body-wrapper::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

.el-table__body-wrapper::-webkit-scrollbar-thumb {
  background: #dcdfe6;
		border-radius: 4px;
}

.el-table__body-wrapper::-webkit-scrollbar-thumb:hover {
  background: #c0c4cc;
}

.el-table__body-wrapper::-webkit-scrollbar-track {
  background: #f5f7fa;
		border-radius: 4px;
}

/* 无头像样式 */
.no-avatar {
  width: 70px;
  height: 70px;
  border-radius: 50%;
  background-color: #f5f7fa;
		display: flex;
  flex-direction: column;
  justify-content: center;
		align-items: center;
  margin: 0 auto;
  border: 1px dashed #dcdfe6;
}

/* 操作按钮样式 */
.operation-buttons {
  display: flex;
  justify-content: center;
  align-items: center;
}

.operation-buttons .el-button {
  padding: 6px 10px;
  margin: 0 4px;
  border-radius: 4px;
  transition: all 0.3s;
		border: none;
  position: relative;
  overflow: hidden;
}

.operation-buttons .el-button::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(255, 255, 255, 0.1);
  border-radius: inherit;
  transform: translateY(100%);
  transition: transform 0.3s;
  z-index: -1;
}

.operation-buttons .el-button:hover::before {
  transform: translateY(0);
}

.operation-buttons .view-btn {
  color: #409EFF;
  background: rgba(64, 158, 255, 0.1);
}

.operation-buttons .view-btn:hover {
  background: rgba(64, 158, 255, 0.2);
  box-shadow: 0 2px 8px rgba(64, 158, 255, 0.2);
  transform: translateY(-2px);
}

.operation-buttons .edit-btn {
  color: #67C23A;
  background: rgba(103, 194, 58, 0.1);
}

.operation-buttons .edit-btn:hover {
  background: rgba(103, 194, 58, 0.2);
  box-shadow: 0 2px 8px rgba(103, 194, 58, 0.2);
  transform: translateY(-2px);
}

.operation-buttons .delete-btn {
  color: #F56C6C;
  background: rgba(245, 108, 108, 0.1);
}

.operation-buttons .delete-btn:hover {
  background: rgba(245, 108, 108, 0.2);
  box-shadow: 0 2px 8px rgba(245, 108, 108, 0.2);
  transform: translateY(-2px);
}

.operation-buttons .el-button i {
  margin-right: 4px;
  font-size: 14px;
}

/* 数据单元格样式 */
.account-info, .name-info, .phone-info, .id-info {
  display: flex;
  align-items: center;
  padding: 0 10px;
}

.avatar-container {
  text-align: center;
}

.avatar-container .el-image {
  transition: all 0.3s;
}

.avatar-container .el-image:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2) !important;
}

/* 标签样式 */
.el-tag {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  font-weight: 500;
		border-radius: 4px;
  padding: 2px 8px;
}

.el-tag--primary {
  background-color: rgba(64, 158, 255, 0.1);
  border-color: rgba(64, 158, 255, 0.2);
}

.el-tag--success {
  background-color: rgba(103, 194, 58, 0.1);
  border-color: rgba(103, 194, 58, 0.2);
}

/* 表格空状态 */
.el-table__empty-block {
  min-height: 120px;
}

.el-table__empty-text {
  color: #909399;
		font-size: 14px;
}

/* 表格加载状态 */
.el-loading-mask {
  background-color: rgba(255, 255, 255, 0.7);
  z-index: 10;
}

/* 新添加的动画效果 */
.search-card .el-input input:focus, 
.search-card .el-select input:focus {
  border-color: #409EFF;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
}

/* 分页全局样式 - 确保生效 */
.custom-pagination {
  border-radius: 20px !important;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.08) !important;
  background: linear-gradient(145deg, #f5f9ff, #ffffff) !important;
  padding: 18px 24px !important;
  width: 96% !important;
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  margin: 30px auto 15px !important;
}

.custom-pagination::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #409EFF, #67C23A, #E6A23C);
  opacity: 1;
}

.custom-pagination .el-pager li {
  min-width: 36px;
  height: 36px;
  line-height: 36px;
		font-size: 14px;
  font-weight: 500;
  border-radius: 50%;
  margin: 0 3px;
  color: #606266;
  background: #f5f7fa;
  border: 1px solid transparent;
  transition: all 0.3s;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.06);
}

.custom-pagination .el-pager li.active {
  color: #fff;
  font-weight: 600;
  background: linear-gradient(145deg, #409EFF, #64b0ff);
  border-color: #409EFF;
  box-shadow: 0 4px 10px rgba(64, 158, 255, 0.3);
}

.custom-pagination .el-pagination__total,
.custom-pagination .el-pagination__jump {
  margin: 0 15px;
  font-weight: 500;
  padding: 0 18px;
  border-radius: 20px;
  height: 36px;
  line-height: 36px;
  font-size: 14px;
  letter-spacing: 0.5px;
  display: flex;
		align-items: center;
}

.custom-pagination .el-pagination__total {
  background: linear-gradient(145deg, #f6f9fe, #ffffff);
  color: #409EFF;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.05);
  border: 1px solid rgba(64, 158, 255, 0.2);
  font-weight: 600;
}

.custom-pagination .btn-prev,
.custom-pagination .btn-next {
  min-width: 36px;
  height: 36px;
  line-height: 36px;
  padding: 0;
  border-radius: 50%;
  font-weight: bold;
  border: 1px solid #e0e0e0;
  background: #fff;
  margin: 0 5px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.06);
}
</style>