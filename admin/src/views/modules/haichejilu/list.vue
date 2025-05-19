<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">还车记录搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-tickets" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">订单编号</label>
							<el-input v-model="searchForm.dingdanbianhao" size="mini" placeholder="请输入订单编号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-truck" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车车牌</label>
							<el-input v-model="searchForm.qichechepai" size="mini" placeholder="请输入汽车车牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#67C23A","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-grid" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#E6A23C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车类型</label>
							<el-input v-model="searchForm.qicheleixing" size="mini" placeholder="请输入汽车类型" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#E6A23C","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}' class="select">
							<i class="el-icon-finished" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#909399"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">是否通过</label>
							<el-select clearable size="mini" v-model="searchForm.sfsh" placeholder="是否通过" :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}'>
								<el-option v-for="(item,index) in sfshOptions" v-bind:key="index" :label="item" :value="item"></el-option>
							</el-select>
						</div>
						<el-button class="search-btn" type="primary" @click="search()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(64, 158, 255, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"32px","fontWeight":"500"}'>
							<i class="el-icon-search" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"32px"}'></i>
							查询
						</el-button>
						<el-button class="reset-btn" @click="resetSearch()" :style='{"border":"1px solid #dcdfe6","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(0, 0, 0, 0.05)","margin":"0 10px 0 0","outline":"none","color":"#606266","borderRadius":"8px","background":"#fff","width":"auto","fontSize":"14px","height":"32px","fontWeight":"500"}'>
							<i class="el-icon-refresh-right" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#606266","height":"32px"}'></i>
							重置
						</el-button>
					</el-row>

					<el-row class="actions" :style='{"padding":"10px 0 0","margin":"10px 0 0","borderTop":"1px dashed #ebeef5","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}'>
						<el-button class="add-btn" v-if="isAuth('haichejilu','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"32px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"32px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('haichejilu','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"32px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"32px"}'></i>
							删除
						</el-button>
						<el-button class="audit-btn" v-if="isAuth('haichejilu','审核')" :disabled="dataListSelections.length?false:true" type="warning" @click="shBatchDialog()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(230, 162, 60, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#E6A23C","width":"auto","fontSize":"14px","height":"32px","fontWeight":"500"}'>
							<i class="el-icon-circle-check" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"32px"}'></i>
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
						<span style="font-size: 16px; color: #333; font-weight: 500;">还车记录列表</span>
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
					v-if="isAuth('haichejilu','查看')"
					:data="dataList"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'500', 'fontSize':'13px', 'background-color':'#f5f7fa', 'padding':'8px 0', 'text-align': 'center'}"
					:cell-style="{'color':'#333', 'fontSize':'13px', 'padding':'8px 0', 'text-align': 'center'}"
					:fit="true">
					<el-table-column :resizable='true' type="selection" align="center" width="40"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="40" align="center" :index="indexMethod"></el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="dingdanbianhao"
						label="订单编号"
						align="center"
						width="85">
						<template slot-scope="scope">
							<div class="order-info" style="justify-content: center;">
								<i class="el-icon-tickets" style="color: #409EFF; margin-right: 3px; font-size: 12px;"></i>
								<span>{{scope.row.dingdanbianhao}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="qichechepai"
						label="汽车车牌"
						align="center"
						width="75">
						<template slot-scope="scope">
							<div class="plate-info" style="justify-content: center;">
								<span>{{scope.row.qichechepai}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="qicheleixing"
						label="汽车类型"
						align="center"
						width="75">
						<template slot-scope="scope">
							<el-tag size="mini" type="info" effect="light">{{scope.row.qicheleixing}}</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="pinpai"
						label="品牌"
						align="center"
						width="65">
						<template slot-scope="scope">
							<div class="brand-info" style="justify-content: center;">
								<span>{{scope.row.pinpai}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="rizujin"
						label="日租金"
						align="center"
						width="60">
						<template slot-scope="scope">
							<div class="price-info" style="justify-content: center;">
								<i class="el-icon-money" style="color: #E6A23C; margin-right: 1px; font-size: 12px;"></i>
								<span>￥{{scope.row.rizujin}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="zucheshijian"
						label="租车时间"
						align="center"
						width="120">
						<template slot-scope="scope">
							<div class="time-info" style="justify-content: center;">
								<i class="el-icon-time" style="color: #909399; margin-right: 3px; font-size: 12px;"></i>
								<span>{{scope.row.zucheshijian}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="haicheshijian"
						label="还车时间"
						align="center"
						width="120">
						<template slot-scope="scope">
							<div class="time-info" style="justify-content: center;">
								<i class="el-icon-time" style="color: #909399; margin-right: 3px; font-size: 12px;"></i>
								<span>{{scope.row.haicheshijian}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column  
						:resizable='true' 
						prop="qichetupian" 
						width="80" 
						label="汽车图片" 
						align="center">
						<template slot-scope="scope">
							<div v-if="scope.row.qichetupian" class="image-container">
								<el-image
									:src="scope.row.qichetupian && scope.row.qichetupian.substring(0,4)=='http' ? 
										(scope.row.qichetupian.split(',w').length>1 ? scope.row.qichetupian : scope.row.qichetupian.split(',')[0]) 
										: $base.url+scope.row.qichetupian.split(',')[0]"
									:preview-src-list="[scope.row.qichetupian && scope.row.qichetupian.substring(0,4)=='http' ? 
										(scope.row.qichetupian.split(',w').length>1 ? scope.row.qichetupian : scope.row.qichetupian.split(',')[0]) 
										: $base.url+scope.row.qichetupian.split(',')[0]]"
									style="width: 50px; height: 50px; border-radius: 4px; object-fit: cover; cursor: pointer; border: 1px solid #fff; box-shadow: 0 1px 5px rgba(0, 0, 0, 0.1);"
								></el-image>
							</div>
							<div v-else class="no-image">
								<i class="el-icon-picture" style="font-size: 16px; color: #909399;"></i>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="yonghuzhanghao"
						label="用户账号"
						align="center"
						width="75">
						<template slot-scope="scope">
							<div class="user-info" style="justify-content: center;">
								<i class="el-icon-user" style="color: #409EFF; margin-right: 2px; font-size: 12px;"></i>
								<span>{{scope.row.yonghuzhanghao}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true' 
						prop="sfsh" 
						label="审核状态"
						align="center"
						width="70">
						<template slot-scope="scope">
							<el-tag v-if="scope.row.sfsh=='否'" type="danger" effect="dark" size="mini">未通过</el-tag>
							<el-tag v-if="scope.row.sfsh=='待审核'" type="warning" effect="dark" size="mini">待审核</el-tag>
							<el-tag v-if="scope.row.sfsh=='是'" type="success" effect="dark" size="mini">已通过</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						width="120" 
						label="操作" 
						align="center">
						<template slot-scope="scope">
							<div class="operation-buttons">
								<el-button type="text" icon="el-icon-view" size="medium" v-if="isAuth('haichejilu','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn" style="color: #409EFF;">查看</el-button>
								<el-button type="text" icon="el-icon-edit" size="medium" v-if="isAuth('haichejilu','修改') && scope.row.sfsh=='待审核'" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn" style="color: #67C23A;">修改</el-button>
								<el-button type="text" icon="el-icon-delete" size="medium" v-if="isAuth('haichejilu','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn" style="color: #F56C6C;">删除</el-button>
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
					style="padding: 10px 15px; margin: 15px auto; white-space: nowrap; color: #606266; text-align: center; background: #ffffff; border-radius: 8px; width: 98%; font-size: 12px; box-shadow: 0 1px 4px rgba(0, 0, 0, 0.05); border: 1px solid #ebeef5;"
				></el-pagination>
			</el-card>
		</template>
		
		<!-- 添加/修改页面  将父组件的search方法传递给子组件-->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>

		<!-- 审核弹窗 -->
		<el-dialog :title="batchIds.length>1?'批量审核':'审核'" :visible.sync="sfshBatchVisiable" width="45%" :style='{"border":"none","borderRadius":"15px","padding":"20px"}'>
			<el-form ref="shBatchForm" :model="shBatchForm" :rules="shRules" label-width="80px">
				<el-form-item label="审核状态" prop="sfsh">
					<el-select v-model="shBatchForm.sfsh" placeholder="审核状态" size="small" :style='{"borderRadius":"4px","height":"36px"}'>
						<el-option label="通过" value="是"></el-option>
						<el-option label="不通过" value="否"></el-option>
						<el-option label="待审核" value="待审核"></el-option>
					</el-select>
				</el-form-item>
				<el-form-item label="内容" prop="shhf">
					<el-input type="textarea" :rows="6" v-model="shBatchForm.shhf" :style='{"borderRadius":"4px"}'></el-input>
				</el-form-item>
			</el-form>
			<span slot="footer" class="dialog-footer">
				<el-button @click="sfshBatchVisiable=false" size="small" :style='{"border":"1px solid #dcdfe6","cursor":"pointer","padding":"0 15px","margin":"0 15px 0 0","outline":"none","color":"#606266","borderRadius":"8px","background":"#fff","width":"auto","fontSize":"14px","height":"32px"}'>取 消</el-button>
				<el-button type="primary" @click="shBatchHandler" size="small" :style='{"border":"0","cursor":"pointer","padding":"0 15px","margin":"0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"32px"}'>确 定</el-button>
			</span>
		</el-dialog>

		<!-- 图片预览弹窗 -->
		<el-dialog title="图片预览" :visible.sync="previewVisible" width="45%" :style='{"border":"none","borderRadius":"15px","padding":"20px"}'>
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
				sfshBatchVisiable:false,
				shBatchForm: {
					ids: [],
					sfsh: '',
					shhf: ''
				},
				shRules: {
					sfsh: [
						{ required: true, message: "审核状态不能为空", trigger: "blur" }
					],
				},
				batchIds: [],
				addOrUpdateFlag:false,
				layouts: ["total","prev","pager","next","sizes","jumper"],
				previewImg: '',
				previewVisible: false,
				tableSize: 'mini',
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
 				if(this.searchForm.dingdanbianhao!='' && this.searchForm.dingdanbianhao!=undefined){
					params['dingdanbianhao'] = '%' + this.searchForm.dingdanbianhao + '%'
				}
 				if(this.searchForm.qichechepai!='' && this.searchForm.qichechepai!=undefined){
					params['qichechepai'] = '%' + this.searchForm.qichechepai + '%'
				}
 				if(this.searchForm.qicheleixing!='' && this.searchForm.qicheleixing!=undefined){
					params['qicheleixing'] = '%' + this.searchForm.qicheleixing + '%'
				}
				if(this.searchForm.sfsh!='' && this.searchForm.sfsh!=undefined){
					params['sfsh'] = this.searchForm.sfsh
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "haichejilu/page",
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
								url: "haichejilu/shBatch?sfsh="+this.shBatchForm.sfsh+"&shhf="+this.shBatchForm.shhf,
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
						url: "haichejilu/delete",
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
			// 表格大小改变
			handleSizeChange(val) {
				this.tableSize = val;
			},

			// 返回序号
			indexMethod(index) {
				return (this.pageIndex - 1) * this.pageSize + index + 1;
			}
		}
	}
</script>
<style lang="css" scoped>
	.main-content .search-card .el-card__body {
		padding: 20px;
	}
	
	.main-content .operation-buttons .el-button {
		margin: 0 5px;
		padding: 4px 8px;
		font-size: 13px;
	}
	
	.main-content .operation-buttons .el-button.view-btn:hover {
		color: #409EFF;
		background: rgba(64, 158, 255, 0.1);
	}
	
	.main-content .operation-buttons .el-button.edit-btn:hover {
		color: #67C23A;
		background: rgba(103, 194, 58, 0.1);
	}
	
	.main-content .operation-buttons .el-button.delete-btn:hover {
		color: #F56C6C;
		background: rgba(245, 108, 108, 0.1);
	}
	
	.main-content .image-container, .main-content .no-image {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
	}
	
	.main-content .custom-pagination .el-pagination__total, 
	.main-content .custom-pagination .el-pagination__jump, 
	.main-content .custom-pagination .el-pagination__sizes {
		margin-right: 10px;
	}
	
	.main-content .custom-pagination .btn-prev, 
	.main-content .custom-pagination .btn-next {
		border-radius: 4px;
		font-weight: 600;
	}
	
	.main-content .custom-pagination .el-pager li {
		border-radius: 4px;
		margin: 0 2px;
		font-weight: 500;
	}
	
	.main-content .custom-pagination .el-pager li.active {
		background-color: #409EFF;
		color: #ffffff;
	}
</style>
