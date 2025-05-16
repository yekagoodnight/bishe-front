<template>
	<div class="main-content" :style='{"width":"100%","padding":"20px 30px","fontSize":"15px","background":"#f5f7fa","borderRadius":"8px"}'>
		<!-- 列表页 -->
		<template v-if="showFlag">
			<!-- 搜索卡片 -->
			<el-card shadow="hover" class="search-card" :style='{"border":"none","padding":"20px","margin":"0 0 25px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"linear-gradient(to right, #ffffff, #f8f9fd)","width":"100%"}'>
				<div class="search-title" style="margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<i class="el-icon-search" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
					<span style="font-size: 16px; color: #333; font-weight: 500;">租赁订单搜索</span>
				</div>
				<el-form class="center-form-pv" :style='{"border":"none","margin":"0","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"flex-start"}' :inline="true" :model="searchForm">
					<el-row :style='{"padding":"0","alignItems":"center","flexWrap":"wrap","background":"none","display":"flex","width":"100%"}' >
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-tickets" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">订单编号</label>
							<el-input v-model="searchForm.dingdanbianhao" size="medium" placeholder="请输入订单编号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-truck" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车车牌</label>
							<el-input v-model="searchForm.qichechepai" size="medium" placeholder="请输入汽车车牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-cooperation" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">汽车类型</label>
							<el-input v-model="searchForm.qicheleixing" size="medium" placeholder="请输入汽车类型" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-flag" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">品牌</label>
							<el-input v-model="searchForm.pinpai" size="medium" placeholder="请输入品牌" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-s-order" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">型号</label>
							<el-input v-model="searchForm.xinghao" size="medium" placeholder="请输入型号" @keydown.enter.native="search()" clearable :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}' prefix-icon="el-icon-search" :prefix-icon-style='{"fontSize":"14px","color":"#409EFF","margin":"0 5px 0 0"}'></el-input>
						</div>
						<div :style='{"alignItems":"center","margin":"0 20px 10px 0","background":"#f5f7fa","padding":"10px 15px","borderRadius":"8px","display":"flex","boxShadow":"0 2px 10px rgba(0, 0, 0, 0.05)","border":"1px solid #ebeef5"}'>
							<i class="el-icon-wallet" :style='{"margin":"0 5px 0 0","fontSize":"16px","color":"#409EFF"}'></i>
							<label :style='{"margin":"0 10px 0 0","whiteSpace":"nowrap","color":"#606266","display":"inline-block","lineHeight":"32px","fontSize":"14px","fontWeight":"500","height":"32px"}' class="item-label">是否支付</label>
							<el-select clearable v-model="searchForm.ispay" placeholder="是否支付" size="medium" :style='{"borderRadius":"4px","border":"none","width":"180px","padding":"0 12px","backgroundColor":"transparent","boxShadow":"none","outline":"none","fontWeight":"normal"}'>
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
						<el-button class="add-btn" v-if="isAuth('zulindingdan','新增')" type="success" @click="addOrUpdateHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(103, 194, 58, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#67C23A","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-plus" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							添加
						</el-button>
						<el-button class="del-btn" v-if="isAuth('zulindingdan','删除')" :disabled="dataListSelections.length?false:true" type="danger" @click="deleteHandler()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(245, 108, 108, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#F56C6C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-delete" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							删除
						</el-button>
						<el-button class="batch-pay-btn" v-if="isAuth('zulindingdan','支付')" :disabled="dataListSelections.length?false:true" type="warning" @click="payBatch()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(230, 162, 60, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#E6A23C","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-wallet" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							批量支付
						</el-button>
						<el-button class="stats-btn" v-if="isAuth('zulindingdan','租赁次数统计')" type="primary" @click="chartDialog1()" :style='{"border":"0","cursor":"pointer","padding":"0 15px","boxShadow":"0 5px 15px rgba(64, 158, 255, 0.2)","margin":"0 10px 0 0","outline":"none","color":"#fff","borderRadius":"8px","background":"#409EFF","width":"auto","fontSize":"14px","height":"36px","fontWeight":"500"}'>
							<i class="el-icon-data-analysis" :style='{"margin":"0 5px 0 0","fontSize":"14px","color":"#fff","height":"36px"}'></i>
							租赁次数统计
						</el-button>
					</el-row>
				</el-form>
			</el-card>
			<!-- 列表卡片 -->
			<el-card shadow="hover" :style='{"border":"none","padding":"20px","borderRadius":"15px","boxShadow":"0 4px 18px 0 rgba(0, 0, 0, 0.1)","background":"#fff","width":"100%"}'>
				<div class="table-header" style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #ebeef5;">
					<div class="table-title" style="display: flex; align-items: center;">
						<i class="el-icon-tickets" style="font-size: 18px; color: #409EFF; margin-right: 8px;"></i>
						<span style="font-size: 16px; color: #333; font-weight: 500;">租赁订单列表</span>
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
					class="tables rental-order-table"
					:stripe='true'
					:size="tableSize"
					:style='{"padding":"0","borderColor":"#ebeef5","borderRadius":"12px","background":"#fff","width":"100%","fontSize":"14px","borderWidth":"1px","borderStyle":"solid"}' 
					:border='true'
					:data="dataList"
					:height="450"
					v-loading="dataListLoading"
					@selection-change="selectionChangeHandler"
					:header-cell-style="{'color':'#333', 'fontWeight':'600', 'fontSize':'14px', 'background-color':'#f5f7fa', 'padding':'12px 8px', 'text-align': 'center', 'border-bottom': '2px solid #dcdfe6', 'transition': 'all 0.2s ease'}"
					:cell-style="cellStyleHandler"
				>
					<el-table-column :resizable='true' type="selection" width="55" align="center"></el-table-column>
					<el-table-column :resizable='true' :sortable='true' label="序号" type="index" width="60" align="center">
						<template slot-scope="scope">
							<el-tag size="medium" type="info" effect="plain">{{scope.$index + 1}}</el-tag>
						</template>
					</el-table-column>
					
					<el-table-column :resizable='true' :sortable='true' prop="dingdanbianhao" label="订单编号" min-width="120" align="center">
						<template slot-scope="scope">
							<div class="order-id">
								<i class="el-icon-tickets" style="color: #409EFF; margin-right: 5px;"></i>
								<span style="font-weight: 500;">{{scope.row.dingdanbianhao}}</span>
							</div>
						</template>
					</el-table-column>
					
					<el-table-column :resizable='true' :sortable='true' label="车辆信息" min-width="140" align="center">
						<template slot-scope="scope">
							<div class="car-info-container">
								<div class="car-image">
									<el-image 
										v-if="scope.row.qichetupian"
										:src="scope.row.qichetupian.substring(0,4)=='http'?scope.row.qichetupian.split(',')[0]:$base.url+scope.row.qichetupian.split(',')[0]" 
										style="width: 60px; height: 60px; border-radius: 5px;"
										fit="cover"
										:preview-src-list="[scope.row.qichetupian.substring(0,4)=='http'?scope.row.qichetupian.split(',')[0]:$base.url+scope.row.qichetupian.split(',')[0]]">
										<div slot="error" class="image-error">
											<i class="el-icon-picture-outline" style="font-size: 20px;"></i>
										</div>
									</el-image>
									<div v-else class="no-image">
										<i class="el-icon-picture-outline" style="font-size: 20px;"></i>
									</div>
								</div>
								<div class="car-plate">
									<el-tag type="primary" effect="plain">{{scope.row.qichechepai}}</el-tag>
								</div>
							</div>
						</template>
					</el-table-column>

					<el-table-column :resizable='true' :sortable='true' prop="yingfujiage" label="价格" min-width="80" align="center">
						<template slot-scope="scope">
							<div class="price-tag">
								<span style="color: #F56C6C; font-weight: 600; font-size: 16px;">¥{{scope.row.yingfujiage}}</span>
							</div>
						</template>
					</el-table-column>
					
					<el-table-column :resizable='true' :sortable='true' label="租期信息" min-width="140" align="center">
						<template slot-scope="scope">
							<div class="rental-info">
								<div class="rental-time">
									<i class="el-icon-time" style="color: #409EFF; margin-right: 4px;"></i>
									<span>{{scope.row.zucheshijian}}</span>
								</div>
								<div class="rental-duration" v-if="scope.row.zulintianshu">
									<span>共 {{scope.row.zulintianshu}} 天</span>
								</div>
							</div>
						</template>
					</el-table-column>
					
					<el-table-column :resizable='true' :sortable='true' label="用户信息" min-width="120" align="center">
						<template slot-scope="scope">
							<div class="user-info">
								<div class="user-account">
									<i class="el-icon-user" style="color: #409EFF; margin-right: 4px;"></i>
									<span>{{scope.row.yonghuzhanghao}}</span>
								</div>
								<div class="user-name">
									<span>{{scope.row.yonghuxingming}}</span>
								</div>
							</div>
						</template>
					</el-table-column>
					
					<el-table-column :resizable='true' :sortable='true' prop="ispay" label="支付状态" min-width="100" align="center">
						<template slot-scope="scope">
							<el-tag type="success" v-if="scope.row.ispay=='已支付'" size="medium">
								<i class="el-icon-check" style="margin-right: 4px;"></i>已支付
							</el-tag>
							<el-tag type="danger" v-else size="medium">
								<i class="el-icon-close" style="margin-right: 4px;"></i>未支付
							</el-tag>
						</template>
					</el-table-column>
					
					<el-table-column width="280" label="操作" align="center" :header-cell-class-name="'action-column-header'">
						<template slot-scope="scope">
							<div style="display: flex; justify-content: center; align-items: center; gap: 8px;">
								<el-button 
									class="action-btn view-button" 
									type="primary" 
									size="mini" 
									icon="el-icon-view"
									@click="addOrUpdateHandler(scope.row.id,'info')"
								>查看</el-button>
								<el-button 
									class="action-btn edit-button" 
									type="warning" 
									size="mini" 
									icon="el-icon-edit"
									@click="addOrUpdateHandler(scope.row.id)"
								>修改</el-button>
								<el-button 
									class="action-btn delete-button" 
									type="danger" 
									size="mini" 
									icon="el-icon-delete"
									@click="deleteHandler(scope.row.id)"
								>删除</el-button>
							</div>
						</template>
					</el-table-column>
				</el-table>
				
				<!-- 分页部分 -->
				<div class="pagination-container">
					<div class="pagination-separator"></div>
					<el-pagination
						@size-change="sizeChangeHandle"
						@current-change="currentChangeHandle"
						:current-page="pageIndex"
						background
						:page-sizes="[10, 20, 50, 100]"
						:page-size="pageSize"
						:layout="layouts.join()"
						:total="totalPage"
						prev-text="上一页"
						next-text="下一页"
						:hide-on-single-page="false"
						class="custom-pagination"
						style="display: inline-flex; padding: 15px 25px; border-radius: 12px; background: linear-gradient(to right, #f8f9fd, #ffffff); box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05); align-items: center;"
					></el-pagination>
				</div>
			</el-card>
		</template>
		
		<!-- 添加/修改页面  将父组件的search方法传递给子组件-->
		<add-or-update v-if="addOrUpdateFlag" :parent="this" ref="addOrUpdate"></add-or-update>

		<haichejilu-cross-add-or-update v-if="haichejiluCrossAddOrUpdateFlag" :parent="this" ref="haichejiluCrossaddOrUpdate"></haichejilu-cross-add-or-update>
		<dingdanquxiao-cross-add-or-update v-if="dingdanquxiaoCrossAddOrUpdateFlag" :parent="this" ref="dingdanquxiaoCrossaddOrUpdate"></dingdanquxiao-cross-add-or-update>
		<guzhangshangbao-cross-add-or-update v-if="guzhangshangbaoCrossAddOrUpdateFlag" :parent="this" ref="guzhangshangbaoCrossaddOrUpdate"></guzhangshangbao-cross-add-or-update>
		<shigushangbao-cross-add-or-update v-if="shigushangbaoCrossAddOrUpdateFlag" :parent="this" ref="shigushangbaoCrossaddOrUpdate"></shigushangbao-cross-add-or-update>
		<cheliangbaoxian-cross-add-or-update v-if="cheliangbaoxianCrossAddOrUpdateFlag" :parent="this" ref="cheliangbaoxianCrossaddOrUpdate"></cheliangbaoxian-cross-add-or-update>



		<el-dialog
			:visible.sync="chartVisiable1"
			width="800">
			<div id="qicheleixingChart1" style="width:100%;height:600px;"></div>
			<span slot="footer" class="dialog-footer">
				<el-button @click="chartDialog1">返回</el-button>
			</span>
		</el-dialog>

		<el-dialog title="预览图" :visible.sync="previewVisible" width="50%">
			<img :src="previewImg" alt="" style="width: 100%;">
		</el-dialog>
	</div>
</template>

<script>
	import * as echarts from 'echarts'
	import chinaJson from "@/components/echarts/china.json";
	import axios from 'axios';
	import AddOrUpdate from "./add-or-update";
	import haichejiluCrossAddOrUpdate from "../haichejilu/add-or-update";
	import dingdanquxiaoCrossAddOrUpdate from "../dingdanquxiao/add-or-update";
	import guzhangshangbaoCrossAddOrUpdate from "../guzhangshangbao/add-or-update";
	import shigushangbaoCrossAddOrUpdate from "../shigushangbao/add-or-update";
	import cheliangbaoxianCrossAddOrUpdate from "../cheliangbaoxian/add-or-update";
	import { isAuth } from "@/utils/utils";
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
				chartVisiable1: false,
				line: {"backgroundColor":"transparent","yAxis":{"axisLabel":{"borderType":"solid","rotate":0,"padding":0,"shadowOffsetX":0,"margin":15,"backgroundColor":"transparent","borderColor":"#000","shadowOffsetY":0,"color":"#333","shadowBlur":0,"show":true,"inside":false,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"width":"","fontSize":12,"lineHeight":24,"shadowColor":"transparent","fontWeight":"normal","height":""},"axisTick":{"show":true,"length":5,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"inside":false},"splitLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#666","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"axisLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"splitArea":{"show":false,"areaStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"rgba(25,25,25,0.3)","opacity":1,"shadowBlur":10,"shadowColor":"rgba(0,0,0,.5)"}}},"xAxis":{"axisLabel":{"borderType":"solid","rotate":0,"padding":0,"shadowOffsetX":0,"margin":4,"backgroundColor":"transparent","borderColor":"#000","shadowOffsetY":0,"color":"#333","shadowBlur":0,"show":true,"inside":false,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"width":"","fontSize":12,"lineHeight":24,"shadowColor":"transparent","fontWeight":"normal","height":""},"axisTick":{"show":true,"length":5,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"inside":false},"splitLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":false},"axisLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"splitArea":{"show":false,"areaStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"rgba(25,25,25,.3)","opacity":1,"shadowBlur":10,"shadowColor":"rgba(0,0,0,.5)"}}},"color":["#1a3194","#91cc75","#fac858","#ee6666","#73c0de","#3ba272","#fc8452","#9a60b4","#ea7ccc"],"legend":{"padding":0,"itemGap":10,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"orient":"horizontal","shadowBlur":0,"bottom":"auto","itemHeight":14,"show":true,"icon":"roundRect","itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"inherit","shadowOffsetY":0,"color":"#333","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"transparent"},"right":"auto","top":"auto","borderRadius":0,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"inherit","shadowBlur":0,"width":"auto","type":"inherit","opacity":1,"shadowColor":"transparent"},"left":"right","borderWidth":0,"width":"80%","itemWidth":20,"textStyle":{"textBorderWidth":0,"color":"#333","textShadowColor":"transparent","ellipsis":"...","overflow":"none","fontSize":12,"lineHeight":24,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":500,"textBorderColor":"transparent","textShadowBlur":0},"shadowColor":"rgba(0,0,0,.3)","height":"auto"},"series":{"showSymbol":true,"symbol":"emptyCircle","symbolSize":4},"tooltip":{"backgroundColor":"#123","textStyle":{"color":"#fff"}},"title":{"borderType":"solid","padding":0,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"shadowBlur":0,"bottom":"auto","show":true,"right":"auto","top":"auto","borderRadius":0,"left":"left","borderWidth":0,"textStyle":{"textBorderWidth":0,"color":"#333","textShadowColor":"transparent","fontSize":14,"lineHeight":24,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":600,"textBorderColor":"#666","textShadowBlur":0},"shadowColor":"transparent"},"base":{"animate":false,"interval":2000}},
				bar: {"backgroundColor":"transparent","yAxis":{"axisLabel":{"borderType":"solid","rotate":0,"padding":0,"shadowOffsetX":0,"margin":12,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"color":"#333","shadowBlur":0,"show":true,"inside":false,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"width":"","fontSize":12,"lineHeight":24,"shadowColor":"transparent","fontWeight":"normal","height":""},"axisTick":{"show":true,"length":5,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"inside":false},"splitLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#666","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"axisLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"splitArea":{"show":false,"areaStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"rgba(25,25,25,0.3)","opacity":1,"shadowBlur":10,"shadowColor":"rgba(0,0,0,.5)"}}},"xAxis":{"axisLabel":{"borderType":"solid","rotate":0,"padding":0,"shadowOffsetX":0,"margin":4,"backgroundColor":"transparent","borderColor":"#000","shadowOffsetY":0,"color":"#333","shadowBlur":0,"show":true,"inside":false,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"width":"","fontSize":12,"lineHeight":24,"shadowColor":"transparent","fontWeight":"normal","height":""},"axisTick":{"show":true,"length":5,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"inside":false},"splitLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":false},"axisLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"cap":"butt","color":"#333","shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"rgba(0,0,0,.5)"},"show":true},"splitArea":{"show":false,"areaStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"rgba(25,25,25,.3)","opacity":1,"shadowBlur":10,"shadowColor":"rgba(0,0,0,.5)"}}},"color":["#1a3194","#ecbb45","#ecc872","#da9d0e","#a48130","#6e80ca","#4d64ca","#2b386f"],"legend":{"padding":0,"itemGap":10,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"orient":"horizontal","shadowBlur":0,"bottom":"auto","itemHeight":14,"show":true,"icon":"roundRect","itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"inherit","shadowOffsetY":0,"color":"#333","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"transparent"},"right":"auto","top":"auto","borderRadius":0,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"inherit","shadowBlur":0,"width":"auto","type":"inherit","opacity":1,"shadowColor":"transparent"},"left":"right","borderWidth":0,"width":"80%","itemWidth":20,"textStyle":{"textBorderWidth":0,"color":"#333","textShadowColor":"transparent","ellipsis":"...","overflow":"none","fontSize":12,"lineHeight":24,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":500,"textBorderColor":"transparent","textShadowBlur":0},"shadowColor":"rgba(0,0,0,.3)","height":"auto"},"series":{"barWidth":"auto","itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"#666","shadowOffsetY":0,"color":"","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"#000"},"colorBy":"data","barCategoryGap":"20%"},"tooltip":{"backgroundColor":"#123","textStyle":{"color":"#fff"}},"title":{"borderType":"solid","padding":0,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"shadowBlur":0,"bottom":"auto","show":true,"right":"auto","top":"auto","borderRadius":0,"left":"left","borderWidth":0,"textStyle":{"textBorderWidth":0,"color":"#333","textShadowColor":"transparent","fontSize":14,"lineHeight":24,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":600,"textBorderColor":"#666","textShadowBlur":0},"shadowColor":"transparent"},"base":{"animate":false,"interval":2000}},
				pie: {"tooltip":{"backgroundColor":"#123","textStyle":{"color":"#fff"}},"backgroundColor":"transparent","color":["#1a3194","#ecbb45","#ecc872","#da9d0e","#a48130","#6e80ca","#4d64ca","#2b386f"],"title":{"borderType":"solid","padding":[5,0,0,0],"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"shadowBlur":0,"bottom":"auto","show":true,"right":"auto","top":"auto","borderRadius":0,"left":"left","borderWidth":0,"textStyle":{"textBorderWidth":0,"color":"#333","textShadowColor":"transparent","fontSize":14,"lineHeight":14,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":600,"textBorderColor":"#666","textShadowBlur":0},"shadowColor":"transparent"},"legend":{"padding":[5,0,0,0],"itemGap":10,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#666","shadowOffsetY":0,"orient":"horizontal","shadowBlur":0,"bottom":"auto","itemHeight":2,"show":true,"icon":"roundRect","itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"inherit","shadowOffsetY":0,"color":"inherit","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"transparent"},"right":0,"top":"auto","borderRadius":0,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"inherit","shadowBlur":0,"width":"auto","type":"inherit","opacity":1,"shadowColor":"transparent"},"left":"right","borderWidth":0,"width":"80%","itemWidth":2,"textStyle":{"textBorderWidth":0,"color":"inherit","textShadowColor":"transparent","ellipsis":"...","overflow":"none","fontSize":12,"lineHeight":12,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":500,"textBorderColor":"transparent","textShadowBlur":0},"shadowColor":"rgba(0,0,0,.3)","height":"auto"},"series":{"itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"#666","shadowOffsetY":0,"color":"","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"#000"},"label":{"borderType":"solid","rotate":0,"padding":0,"textBorderWidth":0,"backgroundColor":"transparent","borderColor":"#666","color":"inherit","show":true,"textShadowColor":"transparent","distanceToLabelLine":5,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"fontSize":12,"lineHeight":18,"textShadowOffsetX":0,"position":"outside","textShadowOffsetY":0,"textBorderType":"solid","textBorderColor":"#666","textShadowBlur":0},"labelLine":{"show":true,"length":10,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"#000"},"length2":14,"smooth":false}}},
				funnel: {"tooltip":{"backgroundColor":"#123","textStyle":{"color":"#fff"}},"backgroundColor":"transparent","color":["#1a3194","#ecbb45","#ecc872","#da9d0e","#a48130","#6e80ca","#4d64ca","#2b386f"],"title":{"borderType":"solid","padding":2,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#ccc","shadowOffsetY":0,"shadowBlur":0,"bottom":"auto","show":true,"right":"auto","top":"auto","borderRadius":0,"left":"center","borderWidth":0,"textStyle":{"textBorderWidth":0,"color":"#666","textShadowColor":"transparent","fontSize":14,"lineHeight":12,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":500,"textBorderColor":"#ccc","textShadowBlur":0},"shadowColor":"transparent"},"legend":{"padding":5,"itemGap":10,"shadowOffsetX":0,"backgroundColor":"transparent","borderColor":"#ccc","shadowOffsetY":0,"orient":"vertical","shadowBlur":0,"bottom":"auto","itemHeight":2,"show":true,"icon":"roundRect","itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"inherit","shadowOffsetY":0,"color":"inherit","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"transparent"},"top":"auto","borderRadius":0,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"color":"inherit","shadowBlur":0,"width":"auto","type":"inherit","opacity":1,"shadowColor":"transparent"},"left":"left","borderWidth":0,"width":"auto","itemWidth":2,"textStyle":{"textBorderWidth":0,"color":"inherit","textShadowColor":"transparent","ellipsis":"...","overflow":"none","fontSize":12,"lineHeight":20,"textShadowOffsetX":0,"textShadowOffsetY":0,"textBorderType":"solid","fontWeight":500,"textBorderColor":"transparent","textShadowBlur":0},"shadowColor":"rgba(0,0,0,.3)","height":"auto"},"series":{"itemStyle":{"borderType":"solid","shadowOffsetX":0,"borderColor":"#000","shadowOffsetY":0,"color":"","shadowBlur":0,"borderWidth":0,"opacity":1,"shadowColor":"#000"},"label":{"borderType":"solid","rotate":0,"padding":0,"textBorderWidth":0,"backgroundColor":"transparent","borderColor":"#fff","color":"","show":true,"textShadowColor":"transparent","distanceToLabelLine":5,"ellipsis":"...","overflow":"none","borderRadius":0,"borderWidth":0,"fontSize":12,"lineHeight":18,"textShadowOffsetX":0,"position":"outside","textShadowOffsetY":0,"textBorderType":"solid","textBorderColor":"#fff","textShadowBlur":0},"labelLine":{"show":true,"length":10,"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"shadowBlur":0,"width":1,"type":"solid","opacity":1,"shadowColor":"#000"},"length2":14,"smooth":false}}},
				boardBase: {"funnelNum":8,"lineNum":8,"gaugeNum":8,"barNum":8,"pieNum":8},
				gauge: {"tooltip":{"backgroundColor":"#123","textStyle":{"color":"#fff"}},"backgroundColor":"transparent","color":["#1a3194","#ecbb45","#ecc872","#da9d0e","#a48130","#6e80ca","#4d64ca","#2b386f"],"title":{"top":"top","left":"left","textStyle":{"fontSize":14,"lineHeight":24,"color":"#333","fontWeight":600}},"series":{"pointer":{"offsetCenter":[0,"10%"],"icon":"path://M2.9,0.7L2.9,0.7c1.4,0,2.6,1.2,2.6,2.6v115c0,1.4-1.2,2.6-2.6,2.6l0,0c-1.4,0-2.6-1.2-2.6-2.6V3.3C0.3,1.9,1.4,0.7,2.9,0.7z","width":8,"length":"80%"},"axisLine":{"lineStyle":{"shadowOffsetX":0,"shadowOffsetY":0,"opacity":0.5,"shadowBlur":1,"shadowColor":"#000"},"roundCap":true},"anchor":{"show":true,"itemStyle":{"color":"inherit"},"size":18,"showAbove":true},"emphasis":{"disabled":false},"progress":{"show":true,"roundCap":true,"overlap":true},"splitNumber":25,"detail":{"formatter":"{value}","backgroundColor":"inherit","color":"#fff","borderRadius":3,"width":20,"fontSize":12,"height":10},"title":{"fontSize":14},"animation":true}},
				addOrUpdateFlag:false,
				haichejiluCrossAddOrUpdateFlag: false,
				dingdanquxiaoCrossAddOrUpdateFlag: false,
				guzhangshangbaoCrossAddOrUpdateFlag: false,
				shigushangbaoCrossAddOrUpdateFlag: false,
				cheliangbaoxianCrossAddOrUpdateFlag: false,
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
			// 强制DOM更新以确保按钮样式生效
			this.$nextTick(() => {
				setTimeout(() => {
					// 获取所有操作按钮并设置样式
					const buttons = document.querySelectorAll('.action-btn');
					buttons.forEach(button => {
						button.style.visibility = 'visible';
						button.style.opacity = '1';
						button.style.display = 'flex';
						button.style.alignItems = 'center';
						button.style.justifyContent = 'center';
					});
					
					// 获取图标并设置样式
					const icons = document.querySelectorAll('.action-btn i');
					icons.forEach(icon => {
						icon.style.visibility = 'visible';
						icon.style.opacity = '1';
						icon.style.display = 'inline-block';
						icon.style.marginRight = '4px';
					});
				}, 100);
			});
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
			haichejiluCrossAddOrUpdate,
			dingdanquxiaoCrossAddOrUpdate,
			guzhangshangbaoCrossAddOrUpdate,
			shigushangbaoCrossAddOrUpdate,
			cheliangbaoxianCrossAddOrUpdate,
		},
		methods: {
			imgPreView(url){
				this.previewImg = url
				this.previewVisible = true
			},
			contentStyleChange() {
				this.contentPageStyleChange()
			},
			// 表格大小切换
			handleSizeChange(val) {
				this.tableSize = val;
			},
			// 支付处理
			payHandler(row) {
				this.$confirm('确认为订单 '+row.dingdanbianhao+' 支付吗?', '支付确认', {
					confirmButtonText: '确定',
					cancelButtonText: '取消',
					type: 'warning'
				}).then(() => {
					this.$http({
						url: this.$storage.get('sessionTable') + "/update",
						method: "post",
						data: {
							id: row.id,
							ispay: "已支付"
						}
					}).then(({ data }) => {
						if (data && data.code === 0) {
							this.$message({
								message: "支付成功",
								type: "success",
								duration: 1500,
								onClose: () => {
									this.getDataList();
								}
							});
						} else {
							this.$message.error(data.msg);
						}
					});
				});
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
			haichejiluCrossAddOrUpdateHandler(row,type,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue){
				if(crossOptPay=='是'&&row.ispay!='已支付'){
					this.$message({
						message: "请支付完成再操作",
						type: "success",
						duration: 1500,
						onClose: () => {
						}
					});
					return
				}
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.haichejiluCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj',row);
				this.$storage.set('crossTable','zulindingdan');
				this.$storage.set('statusColumnName',statusColumnName);
				this.$storage.set('statusColumnValue',statusColumnValue);
				this.$storage.set('tips',tips);
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
							this.haichejiluCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.haichejiluCrossaddOrUpdate.init(row.id,type);
				});
			},
			dingdanquxiaoCrossAddOrUpdateHandler(row,type,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue){
				if(crossOptPay=='是'&&row.ispay!='已支付'){
					this.$message({
						message: "请支付完成再操作",
						type: "success",
						duration: 1500,
						onClose: () => {
						}
					});
					return
				}
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.dingdanquxiaoCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj',row);
				this.$storage.set('crossTable','zulindingdan');
				this.$storage.set('statusColumnName',statusColumnName);
				this.$storage.set('statusColumnValue',statusColumnValue);
				this.$storage.set('tips',tips);
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
							this.dingdanquxiaoCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.dingdanquxiaoCrossaddOrUpdate.init(row.id,type);
				});
			},
			guzhangshangbaoCrossAddOrUpdateHandler(row,type,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue){
				if(crossOptPay=='是'&&row.ispay!='已支付'){
					this.$message({
						message: "请支付完成再操作",
						type: "success",
						duration: 1500,
						onClose: () => {
						}
					});
					return
				}
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.guzhangshangbaoCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj',row);
				this.$storage.set('crossTable','zulindingdan');
				this.$storage.set('statusColumnName',statusColumnName);
				this.$storage.set('statusColumnValue',statusColumnValue);
				this.$storage.set('tips',tips);
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
							this.guzhangshangbaoCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.guzhangshangbaoCrossaddOrUpdate.init(row.id,type);
				});
			},
			shigushangbaoCrossAddOrUpdateHandler(row,type,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue){
				if(crossOptPay=='是'&&row.ispay!='已支付'){
					this.$message({
						message: "请支付完成再操作",
						type: "success",
						duration: 1500,
						onClose: () => {
						}
					});
					return
				}
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.shigushangbaoCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj',row);
				this.$storage.set('crossTable','zulindingdan');
				this.$storage.set('statusColumnName',statusColumnName);
				this.$storage.set('statusColumnValue',statusColumnValue);
				this.$storage.set('tips',tips);
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
							this.shigushangbaoCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.shigushangbaoCrossaddOrUpdate.init(row.id,type);
				});
			},
			cheliangbaoxianCrossAddOrUpdateHandler(row,type,crossOptAudit,crossOptPay,statusColumnName,tips,statusColumnValue){
				if(crossOptPay=='是'&&row.ispay!='已支付'){
					this.$message({
						message: "请支付完成再操作",
						type: "success",
						duration: 1500,
						onClose: () => {
						}
					});
					return
				}
				this.showFlag = false;
				this.addOrUpdateFlag = false;
				this.cheliangbaoxianCrossAddOrUpdateFlag = true;
				this.$storage.set('crossObj',row);
				this.$storage.set('crossTable','zulindingdan');
				this.$storage.set('statusColumnName',statusColumnName);
				this.$storage.set('statusColumnValue',statusColumnValue);
				this.$storage.set('tips',tips);
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
							this.cheliangbaoxianCrossAddOrUpdateFlag = false;
							return;
						}
					}
				}
				this.$nextTick(() => {
					this.$refs.cheliangbaoxianCrossaddOrUpdate.init(row.id,type);
				});
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
							url: 'zulindingdan/update',
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


			// 统计接口
			chartDialog1() {
				this.chartVisiable1 = !this.chartVisiable1;
				this.$nextTick(()=>{
					var qicheleixingChart1 = echarts.init(document.getElementById("qicheleixingChart1"),'macarons');
					this.$http({
						url: "zulindingdan/group/qicheleixing",
						method: "get",
					}).then(({ data }) => {
						if (data && data.code === 0) {
							let res = data.data;
							let xAxis = [];
							let yAxis = [];
							let pArray = []
							for(let i=0;i<res.length;i++){
								if(this.boardBase&&i==this.boardBase.barNum){
									break;
								}
								xAxis.push(res[i].qicheleixing);
								yAxis.push(parseFloat((res[i].total)));
								pArray.push({
									value: parseFloat((res[i].total)),
									name: res[i].qicheleixing
								})
							}
							var option = {};
							let titleObj = this.bar.title
							titleObj.text = '租赁次数统计'
							
							const legendObj = this.bar.legend
							let tooltipObj = {trigger: 'item',formatter: '{b} : {c}'}
							tooltipObj = Object.assign(tooltipObj , this.bar.tooltip?this.bar.tooltip:{})
							let xAxisObj = this.bar.xAxis
							xAxisObj.type = 'category'
							xAxisObj.data = xAxis
							
							let yAxisObj = this.bar.yAxis
							yAxisObj.type = 'value'
							let seriesObj = {
								data: yAxis,
								type: 'bar',
							}
							seriesObj = Object.assign(seriesObj , this.bar.series)
							const gridObj = this.bar.grid
							option = {
								backgroundColor: this.bar.backgroundColor,
								color: this.bar.color,
								title: titleObj,
								legend: legendObj,
								grid: gridObj,
								tooltip: tooltipObj,
								xAxis: xAxisObj,
								yAxis: yAxisObj,
								series: [seriesObj]
							};
							// 使用刚指定的配置项和数据显示图表。
							qicheleixingChart1.setOption(option);
							  //根据窗口的大小变动图表
							window.onresize = function() {
								qicheleixingChart1.resize();
							};
						}else{
							this.$message({
								message: data.msg,
								type: "warning",
								duration: 1500,
							})
						}
					});
				})
			},



			init () {
			},
			
			search() {
				this.pageIndex = 1;
				this.getDataList();
			},
			
			// 添加重置搜索方法
			resetSearch() {
				this.searchForm = {
					key: ""
				};
				this.getDataList();
			},
			
			// 索引方法
			indexMethod(index) {
				return (this.pageIndex - 1) * this.pageSize + index + 1;
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
 				if(this.searchForm.pinpai!='' && this.searchForm.pinpai!=undefined){
					params['pinpai'] = '%' + this.searchForm.pinpai + '%'
				}
 				if(this.searchForm.xinghao!='' && this.searchForm.xinghao!=undefined){
					params['xinghao'] = '%' + this.searchForm.xinghao + '%'
				}
				if(this.searchForm.ispay!='' && this.searchForm.ispay!=undefined){
					params['ispay'] = this.searchForm.ispay
				}
				let user = JSON.parse(this.$storage.getObj('userForm'))
				this.$http({
					url: "zulindingdan/page",
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
						url: "zulindingdan/delete",
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
			// 单元格样式处理
			cellStyleHandler({row, column, rowIndex, columnIndex}) {
				// 操作列特殊样式
				if (column.label === '操作') {
					return {
						'color': '#333', 
						'fontSize': '13px', 
						'padding': '2px', 
						'text-align': 'center'
					};
				}
				// 默认样式
				return {
					'color': '#333', 
					'fontSize': '13px', 
					'padding': '10px 8px', 
					'text-align': 'center'
				};
			},
		}

	};
</script>

<style>
.custom-pagination .btn-prev,
.custom-pagination .btn-next,
.custom-pagination .el-pager li {
  background-color: #f8f9fd;
  color: #606266;
  border-radius: 8px;
  margin: 0 4px;
  font-weight: 500;
  transition: all 0.3s ease;
  height: 32px;
  min-width: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  border: 1px solid #ebeef5;
}

.custom-pagination .el-pager li.active {
  background-color: #409EFF;
  color: #fff;
  font-weight: 600;
  box-shadow: 0 4px 8px rgba(64, 158, 255, 0.2);
  border-color: #409EFF;
  transform: translateY(-1px);
}

.custom-pagination .btn-prev:hover,
.custom-pagination .btn-next:hover,
.custom-pagination .el-pager li:hover:not(.active) {
  color: #409EFF;
  background-color: #ecf5ff;
  border-color: #d9ecff;
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.08);
}

.operation-buttons .el-button--text {
  padding: 4px 8px;
  border-radius: 4px;
  transition: all 0.3s;
}

.operation-buttons .el-button--text:hover {
  background-color: rgba(64, 158, 255, 0.1);
}

.view-btn:hover {
  color: #66b1ff !important;
}

.edit-btn:hover {
  color: #eebe77 !important;
}

.delete-btn:hover {
  color: #f78989 !important;
}

.pay-btn:hover {
  color: #f5ce8c !important;
}

.type-info, .order-info, .plate-info, .time-info {
  transition: all 0.3s ease;
}

.el-image {
  transition: all 0.3s ease;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
}

.el-image:hover {
  transform: scale(1.05);
  box-shadow: 0 4px 16px 0 rgba(0, 0, 0, 0.15);
}

.search-card {
  animation: fadeIn 0.5s ease;
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

.custom-pagination {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: all 0.3s;
  padding: 10px 20px;
  border-radius: 12px;
  background: linear-gradient(to right, #f8f9fd, #ffffff);
}

.custom-pagination:hover {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
}

.custom-pagination .el-pagination__total {
  font-weight: 500;
  color: #606266;
  margin-right: 15px;
  font-size: 13px;
  background-color: #f0f2f5;
  padding: 0 10px;
  height: 32px;
  border-radius: 8px;
  line-height: 32px;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.05);
}

.custom-pagination .el-pagination__sizes {
  margin-right: 15px;
}

.custom-pagination .el-pagination__sizes .el-select .el-input {
  margin: 0;
}

.custom-pagination .el-pagination__sizes .el-select .el-input__inner {
  height: 32px;
  line-height: 32px;
  border-radius: 8px;
  background-color: #f8f9fd;
  border: 1px solid #ebeef5;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
  padding-left: 8px;
  padding-right: 8px;
}

.custom-pagination .el-pagination__sizes .el-select .el-input__inner:hover,
.custom-pagination .el-pagination__sizes .el-select .el-input__inner:focus {
  border-color: #409EFF;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
}

.custom-pagination .el-pagination__jump {
  margin-left: 15px;
  font-weight: 500;
  color: #606266;
}

.custom-pagination .el-pagination__editor.el-input {
  width: 50px;
  margin: 0 5px;
}

.custom-pagination .el-pagination__editor.el-input .el-input__inner {
  height: 32px;
  border-radius: 8px;
  text-align: center;
  background-color: #f8f9fd;
  border: 1px solid #ebeef5;
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
  transition: all 0.3s ease;
}

.custom-pagination .el-pagination__editor.el-input .el-input__inner:hover,
.custom-pagination .el-pagination__editor.el-input .el-input__inner:focus {
  border-color: #409EFF;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.2);
}

.custom-pagination .btn-prev,
.custom-pagination .btn-next {
  background-color: #f8f9fd;
  font-weight: 500;
  color: #606266;
  min-width: 70px;
  border-radius: 8px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  position: relative;
  overflow: hidden;
}

.custom-pagination .btn-prev i,
.custom-pagination .btn-next i {
  font-weight: bold;
  font-size: 12px;
  transition: all 0.3s ease;
}

.custom-pagination .btn-prev:hover i {
  animation: moveLeft 0.5s ease;
}

.custom-pagination .btn-next:hover i {
  animation: moveRight 0.5s ease;
}

@keyframes moveLeft {
  0%, 100% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(-2px);
  }
}

@keyframes moveRight {
  0%, 100% {
    transform: translateX(0);
  }
  50% {
    transform: translateX(2px);
  }
}

.pagination-container {
  position: relative;
  margin-top: 30px;
  text-align: center;
  transition: all 0.3s ease;
}

.pagination-container:hover .pagination-separator {
  width: 80px;
  opacity: 1;
}

.pagination-separator {
  position: absolute;
  top: -10px;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 4px;
  background: linear-gradient(to right, #409EFF, #66b1ff);
  border-radius: 2px;
  transition: all 0.3s ease;
  opacity: 0.8;
}

.custom-pagination .el-pagination__sizes .el-select .el-input.is-focus .el-input__inner {
  border-color: #409EFF;
  box-shadow: 0 0 0 2px rgba(64, 158, 255, 0.3);
}
</style>

<style scoped>
.ultra-compact-table >>> .el-table__body td,
.ultra-compact-table >>> .el-table__header th {
  padding: 0 !important;
  margin: 0 !important;
  border-spacing: 0 !important;
  line-height: 1 !important;
}

.ultra-compact-table >>> .el-table__body,
.ultra-compact-table >>> .el-table__header {
  border-collapse: collapse !important;
  margin: 0 !important;
}

.ultra-compact-table >>> .cell {
  padding: 0 !important;
  margin: 0 !important;
  line-height: 1.2 !important;
  overflow: hidden !important;
  white-space: nowrap !important;
}

.ultra-compact-table >>> .op-btn {
  padding: 1px 2px !important;
  margin: 0 1px !important;
  height: auto !important;
  line-height: 1 !important;
}

.ultra-compact-table >>> .el-button [class*="el-icon-"] + span {
  display: none !important;
}

.ultra-compact-table >>> .mini-tag {
  height: 16px !important;
  line-height: 14px !important;
  padding: 0 2px !important;
  font-size: 9px !important;
}

.ultra-compact-table >>> .el-table__row {
  height: 24px !important;
}

.smaller-text {
  font-size: 10px;
}

.smallest-text {
  font-size: 9px;
  color: #606266;
}

/* 新增更紧凑样式 */
.ultra-compact-table >>> .el-table__row {
  height: 20px !important;
}

.ultra-compact-table >>> .el-button.is-circle {
  width: 18px !important;
  height: 18px !important;
  padding: 0 !important;
  font-size: 10px !important;
}

.ultra-compact-table >>> .operation-buttons {
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  gap: 1px !important;
}

.ultra-compact-table >>> .el-table__body td {
  border-bottom: 1px solid #ebeef5 !important;
}

.ultra-compact-table >>> .el-table__header-wrapper th {
  height: 20px !important;
  font-size: 10px !important;
  padding: 0 !important;
}

.ultra-compact-table >>> .el-table__body-wrapper {
  overflow-x: hidden !important;
}

.ultra-compact-table >>> .el-table {
  font-size: 10px !important;
}

/* 自定义滚动条样式 */
.ultra-compact-table >>> ::-webkit-scrollbar {
  width: 4px;
  height: 4px;
}

.ultra-compact-table >>> ::-webkit-scrollbar-thumb {
  background: #dcdfe6;
  border-radius: 4px;
}

.ultra-compact-table >>> ::-webkit-scrollbar-track {
  background: #f5f7fa;
}

.ultra-compact-table >>> .el-image {
  max-width: 35px !important;
  max-height: 35px !important;
}

/* 租赁订单表格样式 */
.rental-order-table {
  margin-bottom: 20px;
}

.rental-order-table .el-table__header th {
  transition: all 0.2s;
}

.car-info-container {
  display: flex;
  align-items: center;
  padding: 5px;
}

.car-image {
  margin-right: 10px;
  width: 60px;
  height: 60px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.no-image, .image-error {
  width: 60px;
  height: 60px;
  background-color: #f5f7fa;
  border-radius: 5px;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #909399;
}

.car-details {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
}

.car-plate {
  margin-bottom: 6px;
}

.car-type-brand {
  margin-bottom: 4px;
  font-weight: 500;
}

.car-model {
  color: #909399;
}

.price-tag {
  padding: 8px;
  background-color: rgba(245, 108, 108, 0.05);
  border-radius: 8px;
}

.rental-info, .user-info {
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.rental-time, .user-account {
  margin-bottom: 4px;
  font-weight: 500;
}

.rental-duration, .user-name {
  color: #606266;
  font-size: 12px;
}

.action-buttons-fixed {
  display: flex !important;
  justify-content: center !important;
  align-items: center !important;
  gap: 12px !important;
  padding: 8px !important;
  width: 100% !important;
  background: transparent !important;
  z-index: 999 !important;
}

.action-buttons-fixed .el-button {
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1) !important;
  position: relative !important;
  border: none !important;
  opacity: 1 !important;
  visibility: visible !important;
  margin: 0 4px !important;
  height: 32px !important;
  width: 32px !important;
  min-height: 32px !important;
  min-width: 32px !important;
}

.action-buttons-fixed .el-button i {
  visibility: visible !important;
  opacity: 1 !important;
  font-size: 14px !important;
  color: #fff !important;
  display: inline-block !important;
  position: relative !important;
  line-height: 1 !important;
}

.action-buttons-fixed .view-button {
  background-color: #409EFF !important;
}

.action-buttons-fixed .edit-button {
  background-color: #E6A23C !important;
}

.action-buttons-fixed .delete-button {
  background-color: #F56C6C !important;
}

.action-buttons .el-button {
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1) !important;
  transition: all 0.3s ease !important;
  position: relative !important;
  border: none !important;
  opacity: 1 !important;
  visibility: visible !important;
  margin: 0 4px !important;
}

.action-buttons .el-button:hover {
  transform: translateY(-3px) !important;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15) !important;
}

.action-buttons .el-button:active {
  transform: translateY(-1px);
  box-shadow: 0 3px 8px rgba(0, 0, 0, 0.12);
}

.action-buttons .view-button {
  background-color: #409EFF;
}

.action-buttons .view-button:hover {
  background-color: #66b1ff;
}

.action-buttons .edit-button {
  background-color: #E6A23C;
}

.action-buttons .edit-button:hover {
  background-color: #ebb563;
}

.action-buttons .delete-button {
  background-color: #F56C6C;
}

.action-buttons .delete-button:hover {
  background-color: #f78989;
}

.action-buttons .el-button::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  border-radius: 50%;
  background: rgba(255, 255, 255, 0.2);
  opacity: 0;
  transform: scale(0.5);
  transition: all 0.3s ease;
}

.action-buttons .el-button:hover::after {
  opacity: 1;
  transform: scale(1);
}

.action-buttons .el-button i {
  visibility: visible !important;
  opacity: 1 !important;
  font-size: 14px !important;
}

.action-buttons .el-button:hover i {
  transform: scale(1.2);
  filter: brightness(1.1);
}

/* 兼容操作按钮样式 - 保留旧类名以防其他地方使用 */
.operation-buttons {
  display: flex;
  justify-content: center;
  gap: 12px;
}

.custom-pagination {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
  transition: all 0.3s;
}

.custom-pagination:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
}

.order-id {
  display: flex;
  align-items: center;
  justify-content: center;
}

/* 响应式样式 */
@media screen and (max-width: 1200px) {
  .car-info-container {
    flex-direction: column;
    align-items: center;
  }
  
  .car-image {
    margin-right: 0;
    margin-bottom: 8px;
  }
  
  .car-details {
    align-items: center;
    text-align: center;
  }
}

.action-column-header {
  font-weight: 700 !important;
  color: #409EFF !important;
  position: relative;
}

.action-column-header::after {
  content: '';
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 3px;
  background: #409EFF;
  border-radius: 3px;
}

.action-btn {
  padding: 6px 12px !important;
  font-size: 12px !important;
  font-weight: 500 !important;
  display: flex !important;
  align-items: center !important;
  justify-content: center !important;
  border-radius: 4px !important;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1) !important;
  transition: all 0.3s ease !important;
  border: none !important;
  min-width: 70px !important;
  letter-spacing: 0.5px !important;
}

.action-btn i {
  margin-right: 5px !important;
  font-size: 14px !important;
  transition: all 0.3s ease !important;
}

.action-btn:hover {
  transform: translateY(-2px) !important;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15) !important;
  filter: brightness(1.1) !important;
}

.action-btn:active {
  transform: translateY(0) !important;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1) !important;
  filter: brightness(0.95) !important;
}

.action-btn.view-button {
  background-color: #409EFF !important;
}

.action-btn.edit-button {
  background-color: #E6A23C !important;
}

.action-btn.delete-button {
  background-color: #F56C6C !important;
}

.action-btn.view-button:hover {
  background-color: #66b1ff !important;
}

.action-btn.edit-button:hover {
  background-color: #ebb563 !important;
}

.action-btn.delete-button:hover {
  background-color: #f78989 !important;
}
</style>
