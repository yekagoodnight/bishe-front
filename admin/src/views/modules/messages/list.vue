<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">留言搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-user" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">用户名</label>
							<el-input v-model="searchForm.username" size="medium" placeholder="请输入用户名" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search"></el-input>
						</div>
						<el-button class="search-btn" type="primary" @click="search()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(64, 158, 255, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-search" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							查询
						</el-button>
						<el-button class="reset-btn" @click="searchForm.username='';search()" :style='{"border":"1px solid #dcdfe6","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(0, 0, 0, 0.05)","margin":"0 10px 0 0","outline":"none","color":"#606266","borderRadius":"8px","background":"#fff","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-refresh-right" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#606266","height":"36px"}'></i>
							重置
						</el-button>
					</el-row>
					<el-row class="actions" :style='{"padding":"10px 0 0","margin":"10px 0 0","borderTop":"1px dashed #ebeef5","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}'>
						<el-button class="add-btn" v-if="isAuth('messages','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('messages','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
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
						<i class="el-icon-message" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">留言列表</span>
						<span class="table-count" style="font-size: 13px; color: #909399; margin-left: 8px;">(共 {{totalPage}} 条记录)</span>
					</div>
					<div class="table-actions">
						<el-tooltip content="刷新" placement="top">
							<el-button size="mini" type="text" icon="el-icon-refresh" circle @click="search()" style="font-size: 16px; color: #909399;"></el-button>
						</el-tooltip>
					</div>
				</div>
				<el-table 
					class="tables"
					:stripe='true'
					:style='{"padding":"0","borderColor":"#ebeef5","borderRadius":"12px","borderWidth":"1px","background":"#fff","width":"100%","fontSize":"14px","borderStyle":"solid"}' 
					:border='false'
					v-if="isAuth('messages','查看')"
					:data="dataList"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'500', 'fontSize':'14px', 'background-color':'#f5f7fa', 'padding':'12px 0', 'text-align': 'center'}"
					:cell-style="{'color':'#333', 'fontSize':'14px', 'padding':'10px 0', 'text-align': 'center'}">
					<el-table-column :resizable='true' type="selection" align="center" width="50"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="60" align="center"></el-table-column>
					<el-table-column :resizable='true' :sortable='true'  prop="username" label="用户名" align="center" min-width="120">
						<template slot-scope="scope">
							<div class="account-info" style="justify-content: center;">
								<span class="account-text">{{scope.row.username}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="content" label="留言内容" align="center">
						<template slot-scope="scope">
							<span class="ql-snow ql-editor" v-html="scope.row.content"></span>
						</template>
					</el-table-column>
					<el-table-column  :resizable='true' prop="cpicture" width="200" label="留言图片" align="center">
						<template slot-scope="scope">
							<div v-if="scope.row.cpicture">
								<img v-if="scope.row.cpicture.substring(0,4)=='http'&&scope.row.cpicture.split(',w').length>1" :src="scope.row.cpicture" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView(scope.row.cpicture)">
								<img v-else-if="scope.row.cpicture.substring(0,4)=='http'" :src="scope.row.cpicture.split(',')[0]" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView(scope.row.cpicture.split(',')[0])">
								<img v-else :src="$base.url+scope.row.cpicture.split(',')[0]" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView($base.url+scope.row.cpicture.split(',')[0])">
							</div>
							<div v-else class="no-avatar">
								<i class="el-icon-picture-outline" style="font-size: 24px; color: #909399;"></i>
								<span style="font-size: 12px; color: #909399; margin-top: 5px;">无图片</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column :resizable='true' :sortable='true' prop="reply" label="回复内容" align="center">
						<template slot-scope="scope">
							<span class="ql-snow ql-editor" v-html="scope.row.reply"></span>
						</template>
					</el-table-column>
					<el-table-column  :resizable='true' prop="rpicture" width="200" label="回复图片" align="center">
						<template slot-scope="scope">
							<div v-if="scope.row.rpicture">
								<img v-if="scope.row.rpicture.substring(0,4)=='http'&&scope.row.rpicture.split(',w').length>1" :src="scope.row.rpicture" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView(scope.row.rpicture)">
								<img v-else-if="scope.row.rpicture.substring(0,4)=='http'" :src="scope.row.rpicture.split(',')[0]" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView(scope.row.rpicture.split(',')[0])">
								<img v-else :src="$base.url+scope.row.rpicture.split(',')[0]" width="100" height="100" style="object-fit: cover; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.08); cursor:pointer;" @click="imgPreView($base.url+scope.row.rpicture.split(',')[0])">
							</div>
							<div v-else class="no-avatar">
								<i class="el-icon-picture-outline" style="font-size: 24px; color: #909399;"></i>
								<span style="font-size: 12px; color: #909399; margin-top: 5px;">无图片</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column width="300" label="操作" align="center">
						<template slot-scope="scope">
							<div class="operation-buttons">
								<el-button type="text" icon="el-icon-view" size="small" v-if="isAuth('messages','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn">查看</el-button>
								<el-button type="text" icon="el-icon-edit" size="small" v-if="isAuth('messages','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn">修改</el-button>
								<el-button type="text" icon="el-icon-chat-dot-round" size="small" v-if="isAuth('messages','回复')" @click="addOrUpdateHandler(scope.row.id,'msg')" class="reply-btn">回复</el-button>
								<el-button type="text" icon="el-icon-delete" size="small" v-if="isAuth('messages','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn">删除</el-button>
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
					:page-sizes="[10, 50, 100, 200]"
					:page-size="pageSize"
					:layout="layouts.join()"
					:total="totalPage"
					prev-text="< "
					next-text="> "
					:hide-on-single-page="false"
					class="custom-pagination"
				></el-pagination>
			</el-card>
		</template>
		
		<!-- 添加/修改页面  将父组件的search方法传递给子组件-->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>

		<el-dialog title="预览图" :visible.sync="previewVisible" width="50%">
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
 				if(this.searchForm.username!='' && this.searchForm.username!=undefined){
					params['username'] = '%' + this.searchForm.username + '%'
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "messages/page",
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
			// 查看评论
			disscussListHandler(id,type) {
				this.$router.push({path:'/discussmessages',query:{refid:id}});
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
						url: "messages/delete",
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
	/* 复制yonghu页面的美化样式 */
	.search-card {
		border: none;
		padding: 20px;
		margin: 0 0 25px;
		border-radius: 15px;
		box-shadow: 0 4px 18px 0 rgba(0, 0, 0, 0.1);
		background: linear-gradient(to right, #ffffff, #f8f9fd);
		width: 100%;
		animation: fadeIn 0.5s ease;
	}
	.search-title {
		margin-bottom: 15px;
		padding-bottom: 10px;
		border-bottom: 1px solid #ebeef5;
	}
	.operation-buttons .el-button {
		margin: 0 5px;
		padding: 4px 8px;
		font-size: 13px;
		border-radius: 8px;
		transition: all 0.3s;
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
	.operation-buttons .reply-btn {
		color: #E6A23C;
		background: rgba(230, 162, 60, 0.1);
	}
	.operation-buttons .reply-btn:hover {
		background: rgba(230, 162, 60, 0.2);
		box-shadow: 0 2px 8px rgba(230, 162, 60, 0.2);
		transform: translateY(-2px);
	}
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
	.no-avatar {
		display: flex;
		flex-direction: column;
		align-items: center;
		justify-content: center;
		height: 100%;
	}
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
	@keyframes fadeIn {
		from {
			opacity: 0;
			transform: translateY(10px);
		}
		to {
			opacity: 1;
			transform: translateY(0);
		}
	}
</style>
