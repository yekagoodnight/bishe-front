<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">车商搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-user" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">车商账号</label>
							<el-input v-model="searchForm.cheshangzhanghao" size="medium" placeholder="请输入车商账号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-notebook-1" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">车商姓名</label>
							<el-input v-model="searchForm.cheshangxingming" size="medium" placeholder="请输入车商姓名" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#67C23A","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}' class="select">
							<i class="el-icon-tickets" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#E6A23C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">是否通过</label>
							<el-select clearable v-model="searchForm.sfsh" placeholder="请选择" size="medium" :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}'>
								<el-option v-for="(item,index) in sfshOptions" v-bind:key="index" :label="item" :value="item"></el-option>
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
						<el-button class="add-btn" v-if="isAuth('cheshang','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('cheshang','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							删除
						</el-button>
						<el-button class="btn18" v-if="isAuth('cheshang','审核')" :disabled="dataListSelections.length?false:true" type="warning" @click="shBatchDialog()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(230, 162, 60, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#E6A23C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-s-check" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							审核
						</el-button>
					</el-row>
				</el-form>
			</el-card>
			
			<!-- 列表卡片 -->
			<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
				<div class="table-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<div class="table-title" style="display: flex; align-items: center;">
						<i class="el-icon-tickets" style="font-size: 18px; color: #67C23A; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">车商列表</span>
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
					v-if="isAuth('cheshang','查看')"
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
						prop="cheshangzhanghao"
						label="车商账号"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="account-info">
								<span class="account-text">{{scope.row.cheshangzhanghao}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="cheshangxingming"
						label="车商姓名"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="name-info">
								<span class="name-text">{{scope.row.cheshangxingming}}</span>
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
							<div class="gender-info">
								<el-tag size="medium" :type="scope.row.xingbie === '男' ? 'primary' : 'success'" effect="light">
									<i :class="scope.row.xingbie === '男' ? 'el-icon-male' : 'el-icon-female'" style="margin-right: 5px;"></i>
									{{scope.row.xingbie}}
								</el-tag>
							</div>
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
							<div class="phone-info">
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
							<div class="id-info">
								<i class="el-icon-document" style="color: #909399; margin-right: 5px;"></i>
								<span>{{scope.row.shenfenzheng}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						prop="zizhiwenjian" 
						label="资质文件" 
						align="center"
						width="100">
						<template slot-scope="scope">
							<div class="file-info">
								<el-button v-if="scope.row.zizhiwenjian" type="text" size="small" @click="download(scope.row.zizhiwenjian)" class="download-btn">
									<i class="el-icon-download" style="font-size: 16px; margin-right: 5px;"></i>下载
								</el-button>
								<span v-else style="color: #909399; font-size: 13px;">无文件</span>
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
							<div class="avatar-info">
								<div v-if="scope.row.touxiang" class="avatar-container">
									<el-image
										:src="scope.row.touxiang && scope.row.touxiang.substring(0,4)=='http' ? 
											(scope.row.touxiang.split(',w').length>1 ? scope.row.touxiang : scope.row.touxiang.split(',')[0]) 
											: $base.url+scope.row.touxiang.split(',')[0]"
										:preview-src-list="[scope.row.touxiang && scope.row.touxiang.substring(0,4)=='http' ? 
											(scope.row.touxiang.split(',w').length>1 ? scope.row.touxiang : scope.row.touxiang.split(',')[0]) 
											: $base.url+scope.row.touxiang.split(',')[0]]"
										class="avatar-image"
									></el-image>
								</div>
								<div v-else class="no-avatar">
									<i class="el-icon-user" style="font-size: 24px; color: #909399;"></i>
									<span style="font-size: 12px; color: #909399; margin-top: 5px;">无头像</span>
								</div>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true' 
						prop="sfsh" 
						label="审核状态" 
						width="100" 
						align="center">
						<template slot-scope="scope">
							<div class="status-info">
								<el-tag v-if="scope.row.sfsh=='否'" type="danger" size="medium" effect="dark">未通过</el-tag>
								<el-tag v-if="scope.row.sfsh=='待审核'" type="warning" size="medium" effect="dark">待审核</el-tag>
								<el-tag v-if="scope.row.sfsh=='是'" type="success" size="medium" effect="dark">已通过</el-tag>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true' 
						prop="shhf" 
						label="审核回复" 
						show-overflow-tooltip
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="reply-info">
								<i class="el-icon-chat-line-square" style="color: #909399; margin-right: 5px;"></i>
								<span>{{scope.row.shhf || '暂无回复'}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						width="180" 
						label="操作" 
						align="center">
						<template slot-scope="scope">
							<div class="operation-buttons">
								<el-button type="text" icon="el-icon-view" size="small" v-if="isAuth('cheshang','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn">查看</el-button>
								<el-button type="text" icon="el-icon-edit" size="small" v-if="isAuth('cheshang','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn">修改</el-button>
								<el-button type="text" icon="el-icon-delete" size="small" v-if="isAuth('cheshang','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn">删除</el-button>
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


		<!-- 审核弹窗 -->
		<el-dialog :title="batchIds.length>1?'批量审核':'审核'" :visible.sync="sfshBatchVisiable" width="50%" :close-on-click-modal="false" custom-class="custom-dialog">
			<el-form ref="shBatchForm" :model="shBatchForm" :rules="shRules" label-width="100px" class="custom-form">
				<el-form-item label="审核状态" prop="sfsh">
					<el-select v-model="shBatchForm.sfsh" placeholder="审核状态">
						<el-option label="通过" value="是"></el-option>
						<el-option label="不通过" value="否"></el-option>
						<el-option label="待审核" value="待审核"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="回复内容" prop="shhf">
					<el-input type="textarea" :rows="6" v-model="shBatchForm.shhf" placeholder="请输入审核回复内容"></el-input>
				</el-form-item>
			</el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="sfshBatchVisiable=false">取 消</el-button>
				<el-button type="primary" @click="shBatchHandler">确 定</el-button>
			</span>
		</el-dialog>



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
				sfshOptions: [],
				dataList: [],
				pageIndex: 1,
				pageSize: 10,
				totalPage: 0,
				dataListLoading: false,
				dataListSelections: [],
				showFlag: true,
				sfshVisiable: false,
				shForm: {},
				sfshBatchVisiable: false,
				shBatchForm: {
					sfsh:'',
					shhf:''
				},
				shRules: {
					sfsh:[{ required: true, message: '审核状态不能为空', trigger: 'blur' },],
					shhf:[{ required: true, message: '审核回复不能为空', trigger: 'blur' },],
				},
				batchIds:[],
				shList: [],
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





			init () {
				this.sfshOptions = "是,否,待审核".split(',');
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
 				if(this.searchForm.cheshangzhanghao!='' && this.searchForm.cheshangzhanghao!=undefined){
					params['cheshangzhanghao'] = '%' + this.searchForm.cheshangzhanghao + '%'
				}
 				if(this.searchForm.cheshangxingming!='' && this.searchForm.cheshangxingming!=undefined){
					params['cheshangxingming'] = '%' + this.searchForm.cheshangxingming + '%'
				}
				if(this.searchForm.xingbie!='' && this.searchForm.xingbie!=undefined){
					params['xingbie'] = this.searchForm.xingbie
				}
				if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
					params['sfsh'] = this.searchForm.sfsh
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "cheshang/page",
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
			//批量审核窗口
			shBatchDialog(){
				for(let x in this.dataListSelections){
					if(this.dataListSelections[x].sfsh&&this.dataListSelections[x].sfsh!='待审核'){
						this.$message.error('存在已审核数据，不能继续操作');
						this.batchIds = []
						return false
					}
					this.batchIds.push(this.dataListSelections[x].id)
				}
				this.shList = this.dataListSelections
				this.sfshBatchVisiable = true
			},
			//批量审核
			shBatchHandler(){
				this.$refs["shBatchForm"].validate(valid => {
					if(valid){
						this.$confirm(`是否${this.batchIds.length>1?'一键审核':'审核'}选中数据?`, "提示", {
							confirmButtonText: "确定",
							cancelButtonText: "取消",
							type: "warning"
						}).then(() => {
							this.$http({
								url: "cheshang/shBatch?sfsh="+this.shBatchForm.sfsh+"&shhf="+this.shBatchForm.shhf,
								method: "post",
								data: this.batchIds
							}).then(async ({ data }) => {
								if (data && data.code === 0) {
									this.$message({
										message: "操作成功",
										type: "success",
										duration: 1500,
										onClose: () => {
											this.getDataList();
											this.sfshBatchVisiable = false
											this.batchIds = []
										}
									});
								} else {
									this.$message.error(data.msg);
								}
							});
						});
					}
				})
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
						url: "cheshang/delete",
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
	
	/* 表格表头样式优化 */
	.el-table /deep/ .el-table__header-wrapper thead {
		color: #333;
		background: #f5f7fa;
		font-weight: 500;
		width: 100%;
	}
	
	.el-table /deep/ .el-table__header-wrapper thead tr {
		background: none;
	}
	
	.el-table /deep/ .el-table__header-wrapper thead tr th {
		padding: 12px 0;
		background: none;
		border-color: #ebeef5;
		border-width: 0 0 1px 0;
		border-style: solid;
		text-align: center;
	}

	.el-table /deep/ .el-table__header-wrapper thead tr th .cell {
		padding: 0;
		word-wrap: normal;
		color: #333;
		white-space: normal;
		font-weight: 500;
		display: flex;
		align-items: center;
		justify-content: center;
		font-size: 14px;
		line-height: 24px;
		text-overflow: ellipsis;
		word-break: break-all;
		width: 100%;
		height: 100%;
	}
	
	/* 表格内容部分样式优化 */
	.el-table /deep/ .el-table__body-wrapper tbody {
		width: 100%;
	}

	.el-table /deep/ .el-table__body-wrapper tbody tr {
		background: #fff;
		height: 60px !important;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td {
		padding: 12px 0;
		border-color: #ebeef5;
		border-width: 0 0 1px 0;
		border-style: solid;
		text-align: center;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr:hover td {
		background: #f5f7fa;
	}
	
	.el-table /deep/ .el-table__body-wrapper tbody tr td .cell {
		padding: 0;
		overflow: hidden;
		word-break: break-all;
		white-space: normal;
		font-size: 14px;
		line-height: 24px;
		text-overflow: ellipsis;
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}
	
	/* 统一表格内容容器样式 */
	.account-info, .name-info, .phone-info, .id-info, .reply-info, .file-info, .status-info, 
	.gender-info, .avatar-info, .operation-buttons {
		display: flex;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}
	
	/* 账号和姓名文本样式 */
	.account-text, .name-text {
		font-weight: 500;
		color: #303133;
		text-shadow: 0 1px 0 rgba(255, 255, 255, 0.5);
		letter-spacing: 0.5px;
	}
	
	/* 操作按钮样式 */
	.operation-buttons {
		width: 100%;
		display: flex;
		justify-content: center;
		align-items: center;
		flex-wrap: nowrap;
		gap: 5px;
	}

	.operation-buttons .el-button {
		padding: 4px 8px;
		margin: 0;
		border-radius: 4px;
		transition: all 0.3s;
		border: none;
		position: relative;
		overflow: hidden;
		flex-shrink: 0;
		min-width: 52px;
		white-space: nowrap;
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

	/* 头像样式优化 */
	.avatar-container {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		width: 100%;
		height: 100%;
	}

	.avatar-container .avatar-image {
		width: 60px;
		height: 60px;
		border-radius: 50%;
		object-fit: cover;
		border: 3px solid #fff;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
		transition: all 0.3s ease;
		background-color: #f5f7fa;
		overflow: hidden;
	}

	.avatar-container .avatar-image:hover {
		transform: scale(1.05);
		box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
		cursor: pointer;
	}

	.avatar-container /deep/ .el-image__inner {
		width: 100%;
		height: 100%;
		border-radius: 50%;
		object-fit: cover;
	}

	.no-avatar {
		width: 60px;
		height: 60px;
		border-radius: 50%;
		background-color: #f5f7fa;
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		border: 1px dashed #dcdfe6;
		margin: 0 auto;
		transition: all 0.3s ease;
	}

	.no-avatar:hover {
		background-color: #eef1f6;
		border-color: #c0c4cc;
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

	/* 分页样式 */
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

	/* 表格滚动条样式 */
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

	/* 审核弹窗样式 */
	.custom-dialog .el-dialog__header {
		background: #f5f7fa;
		padding: 15px 20px;
		border-radius: 8px 8px 0 0;
	}

	.custom-dialog .el-dialog__body {
		padding: 20px 30px;
	}

	.custom-dialog .el-dialog__footer {
		padding: 10px 20px 15px;
		border-top: 1px solid #f0f2f5;
	}

	.custom-form .el-form-item__label {
		font-weight: 500;
	}

	.custom-form .el-textarea__inner {
		border-radius: 8px;
		border-color: #dcdfe6;
		padding: 10px 15px;
	}

	.custom-form .el-textarea__inner:focus {
		border-color: #409EFF;
		box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
	}
</style>
