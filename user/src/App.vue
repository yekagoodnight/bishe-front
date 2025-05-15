<template>
	<router-view></router-view>
</template>

<script>
	export default {
		name: 'App',
		created() {
			
		},
		mounted() {
			// 动态调整搜索表单按钮位置
			this.$nextTick(() => {
				this.adjustSearchForm();
				// 监听路由变化，在页面切换后重新调整
				this.$router.afterEach(() => {
					setTimeout(() => this.adjustSearchForm(), 300);
				});
			});
		},
		methods: {
			adjustSearchForm() {
				const forms = document.querySelectorAll('#scrollView > div.list-preview > form.list-form-pv');
				forms.forEach(form => {
					// 检查是否已经有按钮容器
					if (!form.querySelector('.button-container')) {
						// 创建按钮容器
						const buttonContainer = document.createElement('div');
						buttonContainer.className = 'button-container';
						
						// 移动搜索和添加按钮到容器中
						const searchBtn = form.querySelector('.list-search-btn');
						const addBtn = form.querySelector('.list-add-btn');
						
						if (searchBtn) buttonContainer.appendChild(searchBtn);
						if (addBtn) buttonContainer.appendChild(addBtn);
						
						// 添加容器到表单底部
						form.appendChild(buttonContainer);
					}
				});
			}
		}
	}
</script>

<style>
	* {
		box-sizing: border-box;
	}

	html,
	body {
		width: 100%;
		height: 100%;
		margin: 0;
		padding: 0;
	}

	[v-cloak] {
		display: none
	}

	.el-tabs__item {
		font-size: 18px;
	}

	.el-loading-spinner .el-loading-text {
		color: #4169E1;
	}

	.el-loading-spinner .path {
		stroke: #4169E1;
	}

	.el-breadcrumb {
		line-height: 60px;
		font-size: 18px;
	}

	.el-breadcrumb__inner.is-link {
		font-size: 18px;
	}

	.ql-editor {
		/*height: 500px;*/
	}

	.ql-snow .ql-tooltip[data-mode="link"]::before {
		content: "请输入链接地址:";
	}

	.ql-snow .ql-tooltip.ql-editing a.ql-action::after {
		border-right: 0px;
		content: "保存";
		padding-right: 0px;
	}

	.ql-snow .ql-tooltip[data-mode="video"]::before {
		content: "请输入视频地址:";
	}

	.ql-snow .ql-picker.ql-size .ql-picker-label::before,
	.ql-snow .ql-picker.ql-size .ql-picker-item::before {
		content: "14px";
	}

	.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="small"]::before,
	.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="small"]::before {
		content: "10px";
	}

	.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="large"]::before,
	.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="large"]::before {
		content: "18px";
	}

	.ql-snow .ql-picker.ql-size .ql-picker-label[data-value="huge"]::before,
	.ql-snow .ql-picker.ql-size .ql-picker-item[data-value="huge"]::before {
		content: "32px";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item::before {
		content: "文本";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="1"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="1"]::before {
		content: "标题1";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="2"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="2"]::before {
		content: "标题2";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="3"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="3"]::before {
		content: "标题3";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="4"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="4"]::before {
		content: "标题4";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="5"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="5"]::before {
		content: "标题5";
	}

	.ql-snow .ql-picker.ql-header .ql-picker-label[data-value="6"]::before,
	.ql-snow .ql-picker.ql-header .ql-picker-item[data-value="6"]::before {
		content: "标题6";
	}

	.ql-snow .ql-picker.ql-font .ql-picker-label::before,
	.ql-snow .ql-picker.ql-font .ql-picker-item::before {
		content: "标准字体";
	}

	.ql-snow .ql-picker.ql-font .ql-picker-label[data-value="serif"]::before,
	.ql-snow .ql-picker.ql-font .ql-picker-item[data-value="serif"]::before {
		content: "衬线字体";
	}

	.ql-snow .ql-picker.ql-font .ql-picker-label[data-value="monospace"]::before,
	.ql-snow .ql-picker.ql-font .ql-picker-item[data-value="monospace"]::before {
		content: "等宽字体";
	}
	.breadcrumb-preview {
		border-radius: 0;
		padding: 0px 16% 10px;
		margin: 0px auto;
		background: transparent;
		width: 100%;
	}
	.breadcrumb-preview .el-breadcrumb {
		border-radius: 0 0 10px 10px;
		background: transparent;
		display: flex;
		font-size: 18px;
		line-height: 1;
		justify-content: center;
		
	}
	.breadcrumb-preview .el-breadcrumb .el-breadcrumb__separator {
		margin: 0 20px;
		color: #ccc;
		font-weight: 500;
	}
	.breadcrumb-preview .el-breadcrumb .item1 .el-breadcrumb__inner a {
		color: #AEAEAE;
		display: inline-block;
		line-height: 50px;
	}
	.breadcrumb-preview .el-breadcrumb .item2 .el-breadcrumb__inner a {
		color: #25489C;
		display: inline-block;
		line-height: 50px;
	}
	.breadcrumb-preview .el-breadcrumb .item3 .el-breadcrumb__inner a {
		color: #AEAEAE;
		display: inline-block;
		line-height: 50px;
	}
	.back_box {
		border-radius: 27px;
		padding: 20px 16%;
		margin: 0;
		background: none;
		display: flex;
		width: 100%;
		justify-content: flex-end;
	}
	.back_box .backBtn {
		border: 0;
		cursor: pointer;
		border-radius: 0;
		padding: 0 10px;
		margin: 0;
		outline: none;
		background: #23469A;
		width: auto;
		font-size: 14px;
		line-height: 30px;
		height: 30px;
	}
	.back_box .backBtn .icon {
		margin: 0 4px 0 0;
		color: #fff;
		font-size: 14px;
	}
	.back_box .backBtn .text {
		color: #fff;
	}
	.back_box .backBtn:hover {
		opacity: 0.7;
	}
	.back_box .backBtn:hover .icon {
		color: rgba(255, 255, 255, 1);
	}
	.back_box .backBtn:hover .text {
		color: rgba(255, 255, 255, 1);
	}
	/* 分页样式 */
	#pagination {
		padding: 0;
		margin: 10px auto;
		color: #333;
		white-space: nowrap;
		font-weight: 500;
		width: 100%;
		text-align: center;
		order: 5;
	}
	#pagination.el-pagination .el-pagination__total {
		margin: 0 10px 0 0;
		color: #666;
		font-weight: 400;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	#pagination.el-pagination .btn-prev {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	#pagination.el-pagination .btn-next {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	#pagination.el-pagination .btn-prev:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	#pagination.el-pagination .btn-next:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pager {
		padding: 0;
		margin: 0;
		display: inline-block;
		vertical-align: top;
	}
	
	#pagination.el-pagination .el-pager .number {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #666;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 50%;
		background: #f4f4f5;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pager .number:hover {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 50%;
		background: #1B3E90;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pager .number.active {
		cursor: default;
		padding: 0 4px;
		margin: 0 5px;
		color: #FFF;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 50%;
		background: #1B3E90;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pagination__sizes {
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pagination__sizes .el-input {
		margin: 0 5px;
		width: 100px;
		position: relative;
	}
	
	#pagination.el-pagination .el-pagination__sizes .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 25px 0 8px;
		color: #606266;
		display: inline-block;
		font-size: 13px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pagination__sizes .el-input span.el-input__suffix {
		top: 0;
		position: absolute;
		right: 0;
		height: 100%;
	}
	
	#pagination.el-pagination .el-pagination__sizes .el-input .el-input__suffix .el-select__caret {
		cursor: pointer;
		color: #C0C4CC;
		width: 25px;
		font-size: 14px;
		line-height: 28px;
		text-align: center;
	}
	
	#pagination.el-pagination .el-pagination__jump {
		margin: 0 0 0 24px;
		color: #606266;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	#pagination.el-pagination .el-pagination__jump .el-input {
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
	
	#pagination.el-pagination .el-pagination__jump .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 3px;
		color: #606266;
		display: inline-block;
		font-size: 14px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}

	/* 修改搜索表单布局 */
	#scrollView > div.list-preview > form.list-form-pv {
		display: flex;
		flex-wrap: wrap;
		justify-content: flex-start;
		align-items: flex-start;
		padding: 15px 20px;
		background: #f9f9f9;
		border-radius: 8px;
		margin-bottom: 20px;
	}
	
	#scrollView > div.list-preview > form.list-form-pv .list-item {
		width: calc(25% - 10px);
		margin-right: 10px;
		margin-bottom: 15px;
		display: flex;
		align-items: center;
	}
	
	#scrollView > div.list-preview > form.list-form-pv .list-item .lable {
		white-space: nowrap;
		margin-right: 8px;
		font-weight: 500;
		color: #333;
		min-width: 90px;
	}
	
	#scrollView > div.list-preview > form.list-form-pv .list-item .el-input {
		width: calc(100% - 100px);
	}
	
	/* 按钮容器 */
	#scrollView > div.list-preview > form.list-form-pv .button-container {
		width: 100%;
		display: flex;
		justify-content: center;
		margin-top: 10px;
	}
	
	#scrollView > div.list-preview > form.list-form-pv .list-search-btn,
	#scrollView > div.list-preview > form.list-form-pv .list-add-btn {
		height: 40px;
		margin: 0 10px;
	}
	
	/* 添加清除浮动，确保布局正确 */
	#scrollView > div.list-preview > form.list-form-pv:after {
		content: "";
		display: table;
		clear: both;
	}

	/* 修改分类器UI样式 */
	#scrollView > div.list-preview > div.category-3 {
		padding: 15px 0;
		background: #f9f9f9;
		border-radius: 10px;
		display: flex;
		flex-wrap: wrap;
		width: 100%;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.03);
	}
	
	#scrollView > div.list-preview > div.category-3 .item {
		cursor: pointer;
		border: none;
		border-radius: 30px;
		padding: 8px 20px;
		margin: 5px 10px;
		color: #5D6483;
		background: #fff;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
		display: flex;
		align-items: center;
		transition: all 0.3s ease;
	}
	
	#scrollView > div.list-preview > div.category-3 .item img {
		border-radius: 50%;
		margin: 0 8px 0 0;
		object-fit: cover;
		display: block;
		width: 25px;
		height: 25px;
		box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
	}
	
	#scrollView > div.list-preview > div.category-3 .item .text {
		color: inherit;
		font-size: 14px;
		font-weight: 500;
	}
	
	#scrollView > div.list-preview > div.category-3 .item:hover {
		color: #AD8B44;
		background: linear-gradient(135deg, #FEFAEE, #F6EED7);
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(173, 139, 68, 0.2);
	}
	
	#scrollView > div.list-preview > div.category-3 .item.active {
		color: #fff;
		background: linear-gradient(135deg, #23469A, #3561c5);
		transform: translateY(-2px);
		box-shadow: 0 4px 12px rgba(35, 70, 154, 0.2);
	}

	/* 修改select2分类样式 */
	#scrollView > div.list-preview > div.select2 {
		padding: 15px 0;
	}
	
	#scrollView > div.list-preview > div.select2 .select2-list {
		padding: 10px 0;
		margin: 8px 0;
		border-bottom: 2px solid rgba(27, 62, 144, 0.1);
	}
	
	#scrollView > div.list-preview > div.select2 .select2-list .label {
		padding: 0 15px 0 5px;
		color: #1B3E90;
		font-weight: 500;
	}
	
	#scrollView > div.list-preview > div.select2 .select2-list .item-body .item {
		cursor: pointer;
		border-radius: 20px;
		padding: 0 15px;
		margin: 0 5px;
		color: #5D6483;
		background: #f0f2f5;
		font-size: 14px;
		line-height: 32px;
		transition: all 0.3s ease;
	}
	
	#scrollView > div.list-preview > div.select2 .select2-list .item-body .item:hover {
		color: #fff;
		background: #3561c5;
	}
	
	#scrollView > div.list-preview > div.select2 .select2-list .item-body .item.active {
		color: #fff;
		background: #1B3E90;
	}

	/* 美化排序控制面板 */
	#scrollView > div.list-preview > div.sort_view {
		display: flex;
		padding: 8px 15px;
		margin: 0 0 12px;
		background: #f9f9f9;
		width: 100%;
		border-radius: 8px;
		box-shadow: 0 2px 6px rgba(0, 0, 0, 0.03);
		align-items: center;
		flex-wrap: wrap;
	}
	
	#scrollView > div.list-preview > div.sort_view::before {
		content: "排序:";
		color: #666;
		font-size: 13px;
		font-weight: 500;
		margin-right: 10px;
	}
	
	#scrollView > div.list-preview > div.sort_view .el-button {
		border: 0;
		border-radius: 16px;
		padding: 0 15px;
		margin: 0 8px 0 0;
		background: linear-gradient(to right, #f8f8f8, #eaeaea);
		height: 30px;
		transition: all 0.3s ease;
		box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
	}
	
	#scrollView > div.list-preview > div.sort_view .el-button .icon {
		margin: 0 3px 0 0;
		color: #25489C;
		font-size: 13px;
		line-height: 30px;
		transition: all 0.3s ease;
	}
	
	#scrollView > div.list-preview > div.sort_view .el-button .text {
		color: #25489C;
		font-size: 13px;
		font-weight: 500;
		line-height: 30px;
		transition: all 0.3s ease;
	}
	
	#scrollView > div.list-preview > div.sort_view .el-button:hover {
		background: linear-gradient(135deg, #f6efd6, #E8D6B0);
		transform: translateY(-2px);
		box-shadow: 0 4px 8px rgba(173, 139, 68, 0.15);
	}
	
	/* 激活状态样式 - 点击量排序 */
	#scrollView > div.list-preview > div.sort_view .click-sort-btn.is-active,
	#scrollView > div.list-preview > div.sort_view .click-sort-btn[class*="active"] {
		background: linear-gradient(135deg, #23469A, #3561c5);
		box-shadow: 0 3px 6px rgba(35, 70, 154, 0.2);
	}
	
	#scrollView > div.list-preview > div.sort_view .click-sort-btn.is-active .icon,
	#scrollView > div.list-preview > div.sort_view .click-sort-btn.is-active .text,
	#scrollView > div.list-preview > div.sort_view .click-sort-btn[class*="active"] .icon,
	#scrollView > div.list-preview > div.sort_view .click-sort-btn[class*="active"] .text {
		color: #fff;
	}
	
	/* 激活状态样式 - 收藏数排序 */
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn.is-active,
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn[class*="active"] {
		background: linear-gradient(135deg, #AD8B44, #c9a25d);
		box-shadow: 0 3px 6px rgba(173, 139, 68, 0.2);
	}
	
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn.is-active .icon,
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn.is-active .text,
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn[class*="active"] .icon,
	#scrollView > div.list-preview > div.sort_view .collect-sort-btn[class*="active"] .text {
		color: #fff;
	}

	/* 美化查询按钮 */
	#scrollView > div.list-preview > form.list-form-pv .list-search-btn {
		cursor: pointer;
		border: none;
		border-radius: 20px;
		padding: 0 25px;
		margin: 0 12px 10px;
		color: #fff;
		background: linear-gradient(135deg, #3561c5, #23469A);
		height: 42px;
		font-size: 14px;
		font-weight: 500;
		letter-spacing: 1px;
		transition: all 0.3s ease;
		display: flex;
		align-items: center;
		justify-content: center;
		box-shadow: 0 4px 15px rgba(35, 70, 154, 0.2);
		position: relative;
		overflow: hidden;
	}

	#scrollView > div.list-preview > form.list-form-pv .list-search-btn:hover {
		transform: translateY(-2px);
		box-shadow: 0 6px 20px rgba(35, 70, 154, 0.3);
		background: linear-gradient(135deg, #4070d6, #2c57bd);
	}

	#scrollView > div.list-preview > form.list-form-pv .list-search-btn:active {
		transform: translateY(1px);
		box-shadow: 0 2px 10px rgba(35, 70, 154, 0.2);
	}

	#scrollView > div.list-preview > form.list-form-pv .list-search-btn::before {
		content: "";
		position: absolute;
		top: 0;
		left: -100%;
		width: 100%;
		height: 100%;
		background: linear-gradient(
			120deg,
			transparent,
			rgba(255, 255, 255, 0.2),
			transparent
		);
		transition: 0.5s;
	}

	#scrollView > div.list-preview > form.list-form-pv .list-search-btn:hover::before {
		left: 100%;
	}

	#scrollView > div.list-preview > form.list-form-pv .list-search-btn i {
		margin-right: 8px;
		font-size: 16px;
	}

	/* 添加响应式样式 */
	@media screen and (max-width: 768px) {
		#scrollView > div.list-preview > form.list-form-pv .list-search-btn {
			width: 100%;
			margin: 10px 0;
		}
	}
</style>