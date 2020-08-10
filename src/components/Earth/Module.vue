<template>
	<div class="body-wrap AreaActualDistribution">
		<!--内容区域-->
		<div class="content" :style="{'right':toggleStatus==='close'?'0px':rightPanelWidth+padding+'px'}">
			<div class="top">
				<div class="preposition_spot"></div>{{dateStr}} {{title}}
				<span @click="turnTo" style="float: right;">
					<i class="fa fa-link" aria-hidden="true"></i> 气象实况分布
				</span>
			</div>
			<div id="imgDiv">
			</div>
		</div>
		<!--条件区域-->
		<right-panel v-if='isConfigLoaded' :rightPanelWidth="rightPanelWidth" :toggleStatus="toggleStatus" @togglePanel="onTogglePanel" :isConfig="config.isConfig" :configJson="config">
			<!--习惯配置-->
			<!--<setting-dialog :configJson="config"></setting-dialog>-->
			<!--模块条件区域-->
			<div class="condition-wrap">
				<div class="con-div">

					<con-collapse title="时间选择">
						<radio-group :btns="dataTypes" v-model="dataType" @change="switchHourDay">

						</radio-group>
						<!--<label-switch :label="'时间尺度'" :btns="dataTypes" v-model="dataType" @change="switchHourDay"></label-switch>-->
						
						<div class="custom_3clear_datepicker" >

							<div class="start-date">
								<div class="date-title">
									开始时间:
								</div>
								<el-row class="date-content">
									<el-col class="date-warp" :xs="dataType==='hour'?16:24" :sm="dataType==='hour'?16:24" :md="dataType==='hour'?16:24" :lg="dataType==='hour'?16:24" :xl="dataType==='hour'?16:24">
										<el-date-picker :editable="false" style="width:100%;" v-model="startTime" type="date" placeholder="选择日期" @change="selectDatePicker" :clearable="false" :picker-options="pDateOptions" default-value="">
										</el-date-picker>
									</el-col>
									<el-col class="date-select" :xs="dataType==='hour'?8:0" :sm="dataType==='hour'?8:0" :md="dataType==='hour'?8:0" :lg="dataType==='hour'?8:0" :xl="dataType==='hour'?8:0">
										<el-select v-show="dataType==='hour'" v-model="startHour" placeholder="请选择" @change="selectDatePicker">
											<el-option v-for="item in hourOptions" :key="item.value" :label="item.label" :value="item.value">
											</el-option>
										</el-select>
									</el-col>
								</el-row>
							</div>
							<div class="end-date">
								<div class="date-title">
									结束时间:
								</div>
								<el-row class="date-content">
									<el-col class="date-warp" :xs="dataType==='hour'?16:24" :sm="dataType==='hour'?16:24" :md="dataType==='hour'?16:24" :lg="dataType==='hour'?16:24" :xl="dataType==='hour'?16:24">
										<el-date-picker :editable="false" style="width:100%" v-model="endTime" type="date" placeholder="选择日期" @change="selectDatePicker" :clearable="false" :picker-options="pDateOptions" default-value="">
										</el-date-picker>
									</el-col>
									<el-col class="date-select" :xs="dataType==='hour'?8:0" :sm="dataType==='hour'?8:0" :md="dataType==='hour'?8:0" :lg="dataType==='hour'?8:0" :xl="dataType==='hour'?8:0">
										<el-select v-show="dataType==='hour'" v-model="endHour" placeholder="请选择" @change="selectDatePicker">
											<el-option v-for="item in hourOptions" :key="item.value" :label="item.label" :value="item.value">
											</el-option>
										</el-select>
									</el-col>
								</el-row>
							</div>
						</div>
						<!--<div class="con-form">
							<div class="con-form-label" style="border-right: none ;">
								{{$t("$3Clear.startDate")}}
							</div>
							<div class="con-form-content" style="border: none;">
								<el-row>
									<el-col class="datePickerEL" :xs="dataType==='hour'?16:24" :sm="dataType==='hour'?16:24" :md="dataType==='hour'?16:24" :lg="dataType==='hour'?16:24" :xl="dataType==='hour'?16:24" style="height: 100%;">
										<el-date-picker style="width:100%;" v-model="startTime" type="date" placeholder="选择日期" @change="selectDatePicker" :clearable="false" :picker-options="pDateOptions" default-value="">
										</el-date-picker>
									</el-col>
									<el-col class="selectEl" :xs="dataType==='hour'?8:0" :sm="dataType==='hour'?8:0" :md="dataType==='hour'?8:0" :lg="dataType==='hour'?8:0" :xl="dataType==='hour'?8:0" style="height: 100%;">
										<el-select v-show="dataType==='hour'" v-model="startHour" placeholder="请选择" @change="selectDatePicker">
											<el-option v-for="item in hourOptions" :key="item.value" :label="item.label" :value="item.value">
											</el-option>
										</el-select>
									</el-col>
								</el-row>

							</div>

						</div>
						<div class="con-form">
							<div class="con-form-label" style="border-right: none ;">
								{{$t("$3Clear.endDate")}}
							</div>
							<div class="con-form-content" style="border: none;">
								<el-row>
									<el-col class="datePickerEL" :xs="dataType==='hour'?16:24" :sm="dataType==='hour'?16:24" :md="dataType==='hour'?16:24" :lg="dataType==='hour'?16:24" :xl="dataType==='hour'?16:24" style="height: 100%;">
										<el-date-picker style="width:100%" v-model="endTime" type="date" placeholder="选择日期" @change="selectDatePicker" :clearable="false" :picker-options="pDateOptions" default-value="">
										</el-date-picker>
									</el-col>
									<el-col class="selectEl" :xs="dataType==='hour'?8:0" :sm="dataType==='hour'?8:0" :md="dataType==='hour'?8:0" :lg="dataType==='hour'?8:0" :xl="dataType==='hour'?8:0" style="height: 100%;">
										<el-select v-show="dataType==='hour'" v-model="endHour" placeholder="请选择" @change="selectDatePicker">
											<el-option v-for="item in hourOptions" :key="item.value" :label="item.label" :value="item.value">
											</el-option>
										</el-select>
									</el-col>
								</el-row>
							</div>
						</div>-->
					</con-collapse>

				</div>

				<div class="con-div">
					<con-collapse title="监测区域">

						<radio-group :btns="areas" v-model="area" @change="getImg">

						</radio-group>

					</con-collapse>
				</div>

				<div class="con-div">
					<con-collapse :title='$t("$3Clear.pollutantSelect")'>

						<radio-group :btns="pols" v-model="pol" @change="getImg">

						</radio-group>

					</con-collapse>
				</div>
			</div>

		</right-panel>
	</div>
</template>

<script>
	import moduleJs from './Module.js';
	export default moduleJs;
</script>

<style lang="less">
	@import url(./assets/style/style.css);
	.AreaActualDistribution {
		width: 100%;
		height: 100%;
		font-size: 14px;
		.content {
			position: absolute;
			margin: 0px;
			left: 0;
			top: 0;
			bottom: 0;
			overflow: hidden;
			box-shadow: 0px 9px 8px 0px rgba(0, 0, 0, 0.17);
		}
	.top {
		height: 32px;
		background-color: @theme_gray_A;
		line-height: 31px;
		/*box-shadow: 3px 2px 8px #ddd;*/
		/*color: @theme_color_highlight;*/
		border: 1px solid @border_color_two;
		box-sizing: border-box;
		font-size: 12px;
		color: @font_color_gray_icon;
		span {
			padding-right: 10px;
			font-size: 12px;
			cursor: pointer;
			i {
				color: @theme_color;
			}
		}
		i {
			color: @theme_color_highlight;
		}
		.preposition_spot{
			width: 4px;
			height: 12px;
			border-radius: 2px;
			float: left;
			background: @font_color_icon;
			margin-left: 12px;
			margin-right: 6px;
			margin-top: 10px;
		}
	}
		#imgDiv {
			height: calc(~'100% - 32px');
			height: -moz-calc(~'100% - 32px');
			height: -webkit-calc(~'100% - 32px');
			z-index: 1;
			width: calc(~'100%');
			z-index: 1;
			position: relative;
			box-sizing: border-box;
			border: 1px solid @border_color_two;
			border-top: none;
		}
	}
	
	
</style>