<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">汽车出租搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-flag" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">租赁标题</label>
							<el-input v-model="searchForm.zulinbiaoti" size="medium" placeholder="请输入租赁标题" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-truck" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车车牌</label>
							<el-input v-model="searchForm.qichechepai" size="medium" placeholder="请输入汽车车牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#67C23A","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-location" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#E6A23C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">地区</label>
							<el-input v-model="searchForm.diqu" size="medium" placeholder="请输入地区" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#E6A23C","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-shop" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#F56C6C"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">品牌</label>
							<el-input v-model="searchForm.pinpai" size="medium" placeholder="请输入品牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#F56C6C","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-unfold" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#909399"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">型号</label>
							<el-input v-model="searchForm.xinghao" size="medium" placeholder="请输入型号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#909399","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-date" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">年份</label>
							<el-input v-model="searchForm.nianfen" size="medium" placeholder="请输入年份" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}' class="select">
							<i class="el-icon-s-opportunity" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#67C23A"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">状态</label>
							<el-select clearable v-model="searchForm.zhuangtai" size="medium" placeholder="请选择状态" :style='{"borderRadius":"4px","border":"none","width":"150px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}'>
								<el-option v-for="(item,index) in zhuangtaiOptions" v-bind:key="index" :label="item" :value="item"></el-option>
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
						<el-button class="add-btn" v-if="isAuth('qichechuzu','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('qichechuzu','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							删除
						</el-button>
						<el-button class="stats-btn" v-if="isAuth('qichechuzu','汽车类型统计')" type="warning" @click="chartDialog1()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(230, 162, 60, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#E6A23C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-pie-chart" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							汽车类型统计
						</el-button>
					</el-row>
				</el-form>
			</el-card>
			
			<!-- 列表卡片 -->
			<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
				<div class="table-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<div class="table-title" style="display: flex; align-items: center;">
						<i class="el-icon-tickets" style="font-size: 18px; color: #67C23A; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">汽车出租列表</span>
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
					:style='{"padding":"0","borderColor":"#ebeef5","borderRadius":"12px","borderWidth":"1px","background":"#fff","width":"100%","fontSize":"14px","borderStyle":"solid", "boxShadow": "0 2px 12px 0 rgba(0, 0, 0, 0.05)"}' 
					:border='false'
					v-if="isAuth('qichechuzu','查看')"
					:data="dataList"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'500', 'fontSize':'14px', 'background-color':'#f5f7fa', 'padding':'10px 0', 'text-align': 'center'}"
					:cell-style="{'color':'#333', 'fontSize':'13px', 'padding':'8px 0', 'text-align': 'center', 'white-space': 'nowrap'}"
				>
					<el-table-column :resizable='true' type="selection" align="center" width="40"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="50" align="center" :index="indexMethod"></el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="zulinbiaoti"
						label="租赁标题"
						align="center"
						min-width="120">
						<template slot-scope="scope">
							<div class="title-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-s-flag" style="color: #409EFF; margin-right: 5px;"></i>
								<span style="white-space: nowrap; overflow: hidden; text-overflow: ellipsis; max-width: 150px;">{{scope.row.zulinbiaoti}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="qichechepai"
						label="汽车车牌"
						align="center"
						width="100">
						<template slot-scope="scope">
							<div class="plate-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-truck" style="color: #67C23A; margin-right: 5px;"></i>
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
						width="90">
						<template slot-scope="scope">
							<el-tag size="small" type="info" effect="plain">
								{{scope.row.qicheleixing}}
							</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="diqu"
						label="地区"
						align="center"
						width="80">
						<template slot-scope="scope">
							<div class="location-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-location" style="color: #E6A23C; margin-right: 3px;"></i>
								<span>{{scope.row.diqu}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="pinpai"
						label="品牌"
						align="center"
						width="80">
						<template slot-scope="scope">
							<el-tag size="small" type="primary" effect="light">
								{{scope.row.pinpai}}
							</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="xinghao"
						label="型号"
						align="center"
						width="80">
						<template slot-scope="scope">
							<span>{{scope.row.xinghao}}</span>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="nianfen"
						label="年份"
						align="center"
						width="70">
						<template slot-scope="scope">
							<span>{{scope.row.nianfen}}</span>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="zhuangtai"
						label="状态"
						align="center"
						width="80">
						<template slot-scope="scope">
							<el-tag size="small" :type="scope.row.zhuangtai === '待租赁' ? 'success' : 'warning'" effect="dark">
								{{scope.row.zhuangtai}}
							</el-tag>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="rizujin"
						label="日租金"
						align="center"
						width="80">
						<template slot-scope="scope">
							<div class="price-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-money" style="color: #F56C6C; margin-right: 3px;"></i>
								<span style="color: #F56C6C; font-weight: 500;">¥{{scope.row.rizujin}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column  
						:resizable='true' 
						prop="qichetupian" 
						label="汽车图片"
						align="center"
						width="70">
						<template slot-scope="scope">
							<div v-if="scope.row.qichetupian" class="car-image-container">
								<el-image
									:src="scope.row.qichetupian.substring(0,4)=='http' ? 
										(scope.row.qichetupian.split(',w').length>1 ? scope.row.qichetupian : scope.row.qichetupian.split(',')[0]) 
										: $base.url+scope.row.qichetupian.split(',')[0]"
									:preview-src-list="[scope.row.qichetupian.substring(0,4)=='http' ? 
										(scope.row.qichetupian.split(',w').length>1 ? scope.row.qichetupian : scope.row.qichetupian.split(',')[0]) 
										: $base.url+scope.row.qichetupian.split(',')[0]]"
									class="car-thumbnail"
								></el-image>
							</div>
							<div v-else class="no-image" style="display: flex; flex-direction: column; align-items: center; justify-content: center;">
								<i class="el-icon-picture" style="font-size: 14px; color: #909399;"></i>
								<span style="font-size: 12px; color: #909399;">无</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="cheshangzhanghao"
						label="车商账号"
						align="center"
						width="100">
						<template slot-scope="scope">
							<div class="merchant-info" style="display: flex; align-items: center; justify-content: center;">
								<i class="el-icon-user" style="color: #909399; margin-right: 3px;"></i>
								<span>{{scope.row.cheshangzhanghao}}</span>
							</div>
						</template>
					</el-table-column>
					<el-table-column 
						:resizable='true' 
						:sortable='true'  
						prop="cheshangxingming"
						label="车商姓名"
						align="center"
						width="100">
						<template slot-scope="scope">
							<span>{{scope.row.cheshangxingming}}</span>
						</template>
					</el-table-column>
					<el-table-column 
						width="160" 
						label="操作"
						align="center">
						<template slot-scope="scope">
							<div class="operation-buttons">
								<el-button type="text" icon="el-icon-view" size="mini" v-if="isAuth('qichechuzu','查看')" @click="addOrUpdateHandler(scope.row.id,'info')" class="view-btn">查看</el-button>
								<el-button type="text" icon="el-icon-shopping-cart-2" size="mini" v-if="isAuth('qichechuzu','汽车租赁')" @click="zulindingdanCrossAddOrUpdateHandler(scope.row,'cross','','','zhuangtai','已被租赁','已租赁,待租赁'.split(',')[0])" class="rent-btn">租赁</el-button>
								<el-button type="text" icon="el-icon-edit" size="mini" v-if="isAuth('qichechuzu','修改')" @click="addOrUpdateHandler(scope.row.id)" class="edit-btn">修改</el-button>
								<el-button type="text" icon="el-icon-chat-dot-round" size="mini" v-if="isAuth('qichechuzu','查看评论')" @click="disscussListHandler(scope.row.id)" class="comment-btn">评论</el-button>
								<el-button type="text" icon="el-icon-delete" size="mini" v-if="isAuth('qichechuzu','删除')" @click="deleteHandler(scope.row.id)" class="delete-btn">删除</el-button>
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

		<zulindingdan-cross-add-or-update v-if="zulindingdanCrossAddOrUpdateFlag" :parent="this" ref="zulindingdanCrossaddOrUpdate"></zulindingdan-cross-add-or-update>

		<!-- 统计图表弹窗 -->
		<el-dialog
			title="汽车类型统计"
			:visible.sync="chartVisiable1"
			width="800px"
			:before-close="handleDialogClose"
			:style='{"borderRadius":"15px","padding":"20px"}'>
			<div id="qicheleixingChart1" style="width:100%;height:600px;"></div>
			<span slot="footer" class="dialog-footer">
				<el-button type="primary" @click="chartDialog1" :style='{"border":"0","cursor":"pointer","padding":"0 20px","boxShadow":"0 5px 15px rgba(0, 0, 0, 0.1)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>关闭</el-button>
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
	import * as echarts from 'echarts';
	import zulindingdanCrossAddOrUpdate from "../zulindingdan/add-or-update";
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
				zulindingdanCrossAddOrUpdateFlag: false,
				layouts: ["total","prev","pager","next","sizes","jumper"],
				previewImg: '',
				previewVisible: false,
				chartVisiable1: false,
				tableSize: 'medium',
				zhuangtaiOptions: ["待租赁","已租赁"],
			};
		},
		components: {
			AddOrUpdate,
			zulindingdanCrossAddOrUpdate
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
			
			// 关闭弹窗
			handleDialogClose() {
				this.previewVisible = false;
				this.chartVisiable1 = false;
			},
			
			// 初始化方法
			init() {
				this.zhuangtaiOptions = ["已租赁","待租赁"];
			},
			
			// 搜索方法
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
				if(this.searchForm.zulinbiaoti!='' && this.searchForm.zulinbiaoti!=undefined){
					params['zulinbiaoti'] = '%' + this.searchForm.zulinbiaoti + '%'
				}
				if(this.searchForm.qichechepai!='' && this.searchForm.qichechepai!=undefined){
					params['qichechepai'] = '%' + this.searchForm.qichechepai + '%'
				}
				if(this.searchForm.qicheleixing!='' && this.searchForm.qicheleixing!=undefined){
					params['qicheleixing'] = this.searchForm.qicheleixing
				}
				if(this.searchForm.diqu!='' && this.searchForm.diqu!=undefined){
					params['diqu'] = '%' + this.searchForm.diqu + '%'
				}
				if(this.searchForm.pinpai!='' && this.searchForm.pinpai!=undefined){
					params['pinpai'] = '%' + this.searchForm.pinpai + '%'
				}
				if(this.searchForm.xinghao!='' && this.searchForm.xinghao!=undefined){
					params['xinghao'] = '%' + this.searchForm.xinghao + '%'
				}
				if(this.searchForm.nianfen!='' && this.searchForm.nianfen!=undefined){
					params['nianfen'] = '%' + this.searchForm.nianfen + '%'
				}
				if(this.searchForm.zhuangtai!='' && this.searchForm.zhuangtai!=undefined){
					params['zhuangtai'] = this.searchForm.zhuangtai
				}
				this.$http({
					url: "qichechuzu/page",
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
			addOrUpdateHandler(id, type) {
				this.showFlag = false;
				this.addOrUpdateFlag = true;
				this.crossAddOrUpdateFlag = false;
				if(type!='info'&&type!='msg'){
					type = 'else';
				}
				this.$nextTick(() => {
					this.$refs.addOrUpdate.init(id, type);
				});
			},
			
			// 查看评论
			disscussListHandler(id) {
				this.$router.push({path:'/discussqichechuzu',query:{refid:id}});
			},
			
			// 汽车租赁处理
			zulindingdanCrossAddOrUpdateHandler(row, type, crossOptAudit, crossOptPay, statusColumnName, tips, statusColumnValue) {
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.zulindingdanCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj', row);
				this.$storage.set('crossTable', 'qichechuzu');
				this.$storage.set('statusColumnName', statusColumnName);
				this.$storage.set('statusColumnValue', statusColumnValue);
				this.$storage.set('tips', tips);
				if(statusColumnName!=''&&!statusColumnName.startsWith("[")) {
					var obj = this.$storage.getObj('crossObj');
					for (var o in obj){
						if(o==statusColumnName && obj[o]==statusColumnValue){
							this.$message({
								message: tips,
								type: "success",
								duration: 1500,
								onClose: () => {
									this.getDataList();
								}
							});
							this.showFlag = true;
							this.zulindingdanCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.zulindingdanCrossaddOrUpdate.init(row.id, type);
				});
			},
			
			// 图片预览
			imgPreView(url) {
				this.previewImg = url;
				this.previewVisible = true;
			},
			
			// 下载
			download(file) {
				let arr = file.replace(new RegExp('upload/', "g"), "");
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
					}));
					const a = document.createElement('a');
					a.href = objectUrl;
					a.download = arr;
					a.dispatchEvent(new MouseEvent('click', {
						bubbles: true,
						cancelable: true,
						view: window
					}));
					window.URL.revokeObjectURL(data);
				}).catch(err => {
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
						}));
						const a = document.createElement('a');
						a.href = objectUrl;
						a.download = arr;
						a.dispatchEvent(new MouseEvent('click', {
							bubbles: true,
							cancelable: true,
							view: window
						}));
						window.URL.revokeObjectURL(data);
					});
				});
			},
			
			// 删除处理
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
						url: "qichechuzu/delete",
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
			
			// 图表展示
			chartDialog1() {
				this.chartVisiable1 = !this.chartVisiable1;
				this.$nextTick(() => {
					let qicheleixingChart1 = echarts.init(document.getElementById("qicheleixingChart1"));
					this.$http({
						url: "qichechuzu/group/qicheleixing",
						method: "get",
					}).then(({ data }) => {
						if (data && data.code === 0) {
							let res = data.data;
							let xAxis = [];
							let yAxis = [];
							let pArray = [];
							for(let i=0; i<res.length; i++){
								xAxis.push(res[i].qicheleixing);
								yAxis.push(parseFloat((res[i].total)));
								pArray.push({
									value: parseFloat((res[i].total)),
									name: res[i].qicheleixing
								});
							}
							
							const option = {
								title: {
									text: '汽车类型统计',
									left: 'center',
									top: 20,
									textStyle: {
										color: '#333',
										fontSize: 16,
										fontWeight: 600
									}
								},
								tooltip: {
									trigger: 'item',
									formatter: '{a} <br/>{b} : {c} ({d}%)'
								},
								legend: {
									orient: 'vertical',
									left: 'left',
									top: 'middle',
									data: xAxis
								},
								series: [
									{
										name: '汽车类型',
										type: 'pie',
										radius: '55%',
										center: ['60%', '50%'],
										data: pArray,
										emphasis: {
											itemStyle: {
												shadowBlur: 10,
												shadowOffsetX: 0,
												shadowColor: 'rgba(0, 0, 0, 0.5)'
											}
										},
										label: {
											normal: {
												formatter: '{b} : {c} ({d}%)'
											}
										}
									}
								]
							};
							
							qicheleixingChart1.setOption(option);
							window.onresize = function() {
								qicheleixingChart1.resize();
							};
						} else {
							this.$message({
								message: data.msg,
								type: "warning",
								duration: 1500,
							});
						}
					});
				});
			}
		}
	};
</script>

<style>
.car-image-container {
	display: flex;
	justify-content: center;
	align-items: center;
	height: 40px;
	width: 100%;
	position: relative;
	overflow: hidden;
}

.car-thumbnail {
	width: 35px !important;
	height: 35px !important;
	border-radius: 4px;
	cursor: default;
	border: 1px solid #ebeef5;
	box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

/* 强制覆盖Element UI的图片样式 */
.el-image__inner {
	width: 100% !important;
	height: 100% !important;
	object-fit: contain !important;
	transition: none !important;
}

/* 完全移除悬停效果 */
.el-image__inner:hover,
.car-thumbnail:hover,
.el-image:hover {
	transform: none !important;
	cursor: default !important;
	transition: none !important;
}

/* 为表格添加斑马纹效果增强视觉感受 */
.tables {
	--el-table-row-hover-bg-color: rgba(0, 0, 0, 0.03);
	--el-table-border-color: #ebeef5;
}

.tables .el-table__row:hover > td {
	background-color: rgba(0, 0, 0, 0.03);
}

/* 美化表格边框和背景 */
.tables th {
	background-color: #f5f7fa !important;
	border-bottom: 1px solid #ebeef5;
}

.tables td {
	border-bottom: 1px solid #ebeef5;
}

/* 操作按钮样式紧凑化 */
.operation-buttons {
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	align-items: center;
	gap: 2px;
}

/* 覆盖预览图片的样式 */
.el-image-viewer__wrapper {
	z-index: 2030;
}

.operation-buttons .el-button {
	margin: 1px;
	padding: 2px 4px;
	font-size: 12px;
}

.operation-buttons .view-btn {
	color: #409EFF;
}

.operation-buttons .rent-btn {
	color: #67C23A;
}

.operation-buttons .edit-btn {
	color: #E6A23C;
}

.operation-buttons .comment-btn {
	color: #909399;
}

.operation-buttons .delete-btn {
	color: #F56C6C;
}

.no-image {
	width: 100%;
	height: 35px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

/* 美化分页器 */
.custom-pagination .el-pagination__total {
	margin-right: 10px;
}

.custom-pagination .el-pagination__sizes {
	margin-left: 10px;
}
</style>
