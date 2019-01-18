
## web-app （监测应用）
>监测应用目前可分为10种，每一种都包含主管和联网端，监测分为当前监测列表和历史监测列表，列表默认显示7天（火警当前列表24小时）报警数据，可对数据进行状态和时间筛选.
### 1.项目地址
>http://fubangyun.com:9001/yang/web-app.git.
### 2.项目安装
```git
$ git clone http://fubangyun.com:9001/yang/web-app.git
```
### 3.项目启动
```git
# install dependencies
npm install
# serve with hot reload at localhost:8082
npm run dev
# build for production with minification
npm run build
# build for longnan		独立部署陇南
npm run alone_build_longnan  
# build for zhongwei		独立部署中卫
npm run alone_build_zhongwei
# run unit tests
npm run unit
# run e2e tests
npm run e2e
# run all tests
npm test
```
### 4.项目目录
4-1.项目结构：<br/>
![avatar](/images/web-app.png)<br/>
4-2.src文件结构：<br/>
![avatar](/images/web-app-src.png)<br/>
4-3.page文件结构：<br/>
```file
|-- page
    |-- directoryList.md
    |-- app_swiper
    |   |-- app_swiper.vue
    |-- common
    |-- device										//设备动作监测
    |-- electricalarm								//电气监测
    |-- error
    |-- faultalarm									//故障监测
    |-- firealarm									//火警监测
    |-- firedoor									//防火门监测
    |-- govhydrant									//市政消火栓监测
    |-- hydrant										//室外消火栓监测
    |-- information
    |   |-- company
    |   |   |-- information_info.vue
    |   |   |-- information_list.vue
    |   |-- responsible
    |       |-- information_edit_res.vue
    |       |-- information_info_res.vue
    |       |-- information_list_res.vue
    |-- news
    |   |-- news_list.vue
    |   |-- news_listInfo.vue
    |-- offsite										//设施离位监测
    |-- perimeter
    |   |-- company
    |       |-- company.vue
    |       |-- company_basedata.js
    |       |-- company_res.vue
    |-- public
    |-- statistics
    |-- wateralarm									//水系统监测
    |-- watercover									//水浸监测
```
### 5.项目架构图
![avatar](/images/jcyy.png)<br/>
### 6.使用技术
>项目使用框架是vue，响应式UI组件和数据，第三方依赖和插件有：vuex、axios、jquery、vue-router、样式采用less预编译，监测应用列表和详情数据渲染字段采取uiconfig后台可配置，前端解析如下.

```js
app_alarm_list(this.get_grams, this.time_length).then(res => {
		let data = res.data;
		let ui = res.ui;
		console.log('电气监测测', data)
		console.log('电气检测ui', ui)
		let build_jpath = ui[0].data[0].jpath; //建筑名字
		let floor_jpath = ui[0].data[1].jpath; //楼层名字
		let location_jpath = ui[0].data[2].jpath; //位置详情
		let device_jpath = ui[0].data[3].jpath; //部件类型
		let loop_number_jpath = ui[0].data[4].jpath; //回路号
		let component_number_jpath = ui[0].data[5].jpath; //部件号
		let alarm_time_jpath = ui[0].data[6].jpath; //报警时间
		let is_read_jpath = ui[0].data[7].jpath; //报警时间
		let sure_state_jpath = ui[0].data[8].jpath; //确认状态
		let device_state_jpath = ui[0].data[9].jpath; //设备状态
```

## task（维保）
>用于维保单位新建、编辑、接收维保、维修任务及任务状态的追踪、具体任务检查项、检查结果是否合格，查看任务评价；联网单位查看维保、维修任务的状态、具体任务检查项、检查结果是否合格，对任务进行评价；维保单位或者联网单位将当次维保或维修任务的具体情况以报告的形式提交给联网单位或主管部门。
### 1.项目地址
>http://fubangyun.com:9001/front_development/task.git.
### 2.项目安装
```git
$ http://fubangyun.com:9001/front_development/task.git
```
### 3.项目启动
```git
# install dependencies
npm install
# serve with hot reload at localhost:8081
npm run dev
# build for production with minification
npm run build
# build for longnan		独立部署陇南
npm run alone_build_longnan  
# build for zhongwei		独立部署中卫
npm run alone_build_zhongwei
# run unit tests
npm run unit
# run e2e tests
npm run e2e
# run all tests
npm test
```
### 4.项目目录
4-1.项目结构：<br/>
![avatar](/images/wb.png)<br/>
4-2.src文件结构：<br/>
![avatar](/images/wb-src.png)<br/>
4-3.page文件结构：<br/>
```file
|-- page
    |-- common													//公共组件
    |   |-- leftSlider.vue
    |   |-- my_navbar.vue
    |   |-- nav_build_bar.vue
    |   |-- nav_detail_bar.vue
    |   |-- report_bar.vue
    |   |-- slider.vue
    |-- company													//保养记录
    |   |-- task_list.vue
    |-- edit													//任务编辑
    |   |-- add_device.vue
    |   |-- edit_wb_select_device.vue
    |   |-- edit_wb_task.vue
    |   |-- edit_wx_select_device.vue
    |   |-- edit_wx_task.vue
    |-- error
    |   |-- error_nodata.vue
    |   |-- error_nonetwork.vue
    |   |-- error_outtime.vue
    |   |-- no_check.vue
    |   |-- no_data.vue
    |-- main													//维保系统首页及列表
    |   |-- assign_task_list.vue
    |   |-- completed_task_list.vue
    |   |-- main.vue
    |   |-- need_deal_task_list.vue
    |   |-- task_null.vue
    |-- newbuild												//任务新建
    |   |-- build_task.vue
    |   |-- build_wb_task.vue
    |   |-- build_wx_task.vue
    |   |-- select_personleader.vue
    |   |-- select_personnel.vue
    |   |-- select_project.vue
    |-- wb														//维保任务详情，检查项及检查结果
    |   |-- wb_basic_info.vue
    |   |-- wb_check_list.vue
    |   |-- wb_company.vue
    |   |-- wb_info_edit.vue
    |   |-- wb_result_info.vue
    |   |-- write_wb_info.vue
    |-- wbreport												//维保、维修报告
    |   |-- nullPage.vue
    |   |-- preview.vue
    |   |-- remoke.vue
    |   |-- report_building_list.vue
    |   |-- report_list.vue
    |   |-- report_list_res.vue
    |-- wx														//维修任务详情，维修项及维修结果
        |-- look_evaluate.vue
        |-- select_device.vue
        |-- single_select_device.vue
        |-- write_wx_info.vue
        |-- wx_basic_info.vue
        |-- wx_company.vue
        |-- wx_evaluate.vue
```
### 5.项目架构图
![avatar](/images/weibao.png)<br/>
### 6.使用技术
>项目使用框架是vue，响应式UI组件和数据，第三方依赖和插件有：vuex、axios、jquery、vue-router、样式采用scss预编译.

## add_device（设备添加、社区消防、微消防站）
>设备添加：联网单位添加设备（手动和二维码扫描）、设备详情、编辑设备；社区消防：业主，社区监测，建筑列表、新建、详情、编辑，小区列表、详情、编辑；微消防站：创建管理员，添加人员账户、详情、编辑，设备添加、详情、编辑。
### 1.项目地址
>http://fubangyun.com:9001/front_development/add_device.git.
### 2.项目安装
```git
$ http://fubangyun.com:9001/front_development/add_device.git
```
### 3.项目启动
```git
# install dependencies
npm install
# serve with hot reload at localhost:8080
npm run dev
# build for production with minification
npm run build
# build for longnan		独立部署陇南
npm run alone_build_longnan  
# build for zhongwei		独立部署中卫
npm run alone_build_zhongwei
# run unit tests
npm run unit
# run e2e tests
npm run e2e
# run all tests
npm test
```
### 4.项目目录
4-1.项目结构：<br/>
![avatar](/images/device.png)<br/>
4-2.src文件结构：<br/>
![avatar](/images/device-src.png)<br/>
4-3.page文件结构：<br/>
```file
page
    |-- alarm_host										//添加消防主机
    |   |-- add_alarm_host.vue
    |   |-- device_info.vue								//设备详情
    |   |-- edit_alarm_host.vue							//编辑消防主机
    |   |-- uiconfig.js
    |   |-- user_trans_device.vue						//用户传输装置
    |-- alone_device									//独立设备的添加、编辑
    |   |-- add_alone_device.vue
    |   |-- alone_device.js
    |   |-- edit_alone_device.vue
    |-- building										//社区 建筑列表、详情、新建、编辑
    |   |-- building_detail.vue
    |   |-- building_list.vue
    |   |-- build_building.vue
    |   |-- edit_building.vue
    |-- build_owner										//关联业主
    |   |-- building.vue
    |   |-- build_owner.vue
    |   |-- edit_owner.vue
    |   |-- floor.vue
    |   |-- location.vue
    |-- common											//公共组件
    |   |-- add_device.vue
    |   |-- device_null.vue
    |   |-- leftSlider.vue
    |   |-- leftSlider_up.vue
    |   |-- my_navbar.vue
    |   |-- my_navbar_click.vue
    |   |-- no_data.vue
    |   |-- building
    |   |   |-- building.vue
    |   |-- floor
    |   |   |-- floor.vue
    |   |-- protocol
    |   |   |-- protocol.vue
    |   |-- region
    |   |   |-- region.vue
    |   |-- scroll
    |   |   |-- config.js
    |   |   |-- scroll.vue
    |   |   |-- utils.js
    |   |-- system
    |       |-- system.vue
    |-- community							
    |   |-- community_list.vue							//社区监测列表
    |-- detector										//部件添加、编辑
    |   |-- add_detector.vue
    |   |-- detector.js
    |   |-- edit_detector.vue
    |-- error
    |   |-- error_nodata.vue
    |   |-- error_nonetwork.vue
    |   |-- error_outtime.vue
    |   |-- no_check.vue
    |   |-- no_data.vue
    |-- location										//小区列表、详情、编辑
    |   |-- edit_location.vue
    |   |-- location_detail.vue
    |   |-- location_list.vue
    |-- miniFire										//微消防站创建管理员、编辑，添加人员、详情、编辑，设备添加、详情、编辑
    |   |-- account
    |   |   |-- account_detail.vue
    |   |   |-- build_account.vue
    |   |   |-- edit_account.vue
    |   |-- device
    |   |   |-- add_detector.vue
    |   |   |-- add_trans_device.vue
    |   |   |-- device_info.vue
    |   |   |-- device_null.vue
    |   |   |-- edit_detector.vue
    |   |   |-- edit_trans_device.vue
    |   |   |-- question.vue
    |   |   |-- uiconfig.js
    |   |   |-- user_trans_device.vue
    |   |-- register
    |       |-- area.js
    |       |-- base_info.vue
    |       |-- build_admin.vue
    |       |-- edit_base.vue
    |       |-- people.js
    |-- native_notice_info							
    |   |-- alarm_detail.vue							//原生社区通知告警详情
    |   |-- mini_alarm_detail.vue						//原生微消防站通知告警详情
    |-- owner											//业主列表、详情、业主设备列表、设备详情
    |   |-- add_device.vue
    |   |-- alarm_detail.vue
    |   |-- device_electric_info.vue
    |   |-- device_info.vue
    |   |-- instructions.vue
    |   |-- notice_detail.vue
    |   |-- notice_info.vue
    |   |-- owner_detail.vue
    |   |-- owner_list.vue
    |   |-- relieve_relation_info.vue
    |   |-- uiconfig.js
    |-- trans_device
        |-- add_trans_device.vue
        |-- edit_trans_device.vue
        |-- frequency.js
```
### 5.项目架构图
![avatar](/images/add_device-page.png)<br/>
### 6.使用技术
>项目使用框架是vue，响应式UI组件和数据，第三方依赖和插件有：vuex、axios、jquery、vue-router、样式采用scss预编译，数据渲染字段采取uiconfig后台可配置.前端解析如下.

```js
owner_list(this.get_grams).then(res => {
			if(isSearch && isSearch == 1) {
				if(objFlag.uuid != config.uuid) {
					return false;
				}
			}
			console.log(res)
			let ui = res.ui;
			let data = res.data;
			let company_household_id_jpath = ui[0].data[0].jpath; //业主id
			let household_user_name_jpath = ui[0].data[1].jpath; //业名字
			let belong_subdistrict_jpath = ui[0].data[2].jpath; //所属小区
			let building_name_jpath = ui[0].data[3].jpath; //建筑名字
			let floor_name_jpath = ui[0].data[4].jpath; //楼层 name
			let unit_id_jpath = ui[0].data[5].jpath; //单元号
			let room_id_jpath = ui[0].data[6].jpath; //房间号
			let bind_status_jpath = ui[0].data[7].jpath; //绑定状态
			let bind_id_jpath = ui[0].data[8].jpath; //绑定id
			let house_phone_jpath = ui[0].data[9].jpath; //业主电话
```
## fire_notice（消防资讯、消防常识）
>包括：消防资讯和常识列表、详情。
### 1.项目地址
>http://fubangyun.com:9001/fuleiyang/fire_notice.git.
### 2.项目安装
```git
$ http://fubangyun.com:9001/fuleiyang/fire_notice.git
```
### 3.项目启动
```git
# install dependencies
npm install
# serve with hot reload at localhost:8082
npm run dev
# build for production with minification
npm run build
# build for longnan		独立部署陇南
npm run alone_build_longnan  
# build for zhongwei		独立部署中卫
npm run alone_build_zhongwei
# run unit tests
npm run unit
# run e2e tests
npm run e2e
# run all tests
npm test
```
### 4.项目目录
4-1.项目结构：<br/>
![avatar](/images/fire_notice.png)<br/>
4-2.src文件结构：<br/>
![avatar](/images/fire-notice-src.png)<br/>
4-3.page文件结构：<br/>
```file
|-- pages
    |-- news
        |-- common_sense.vue
        |-- detail.vue
        |-- new.vue
```
### 5.项目架构图
![avatar](/images/fire-notice.png)<br/>
### 6.使用技术
>项目使用框架是vue，响应式UI组件和数据，第三方依赖和插件有：vuex、axios、jquery、vue-router、样式采用scss预编译.

## webApp（通知二级列表、应用详情）
>app通知二级列表：包括告警应用监测、社区监测、维保维修通知。
### 1.项目地址
>http://fubangyun.com:9001/front_development/webApp.git.
### 2.项目安装
```git
$ http://fubangyun.com:9001/front_development/webApp.git
```
### 3.项目启动
```git
# install dependencies
npm install
# serve with hot reload at localhost:8082
npm run dev
# build for production with minification
npm run build
# build for longnan		独立部署陇南
npm run alone_build_longnan  
# build for zhongwei		独立部署中卫
npm run alone_build_zhongwei
# run unit tests
npm run unit
# run e2e tests
npm run e2e
# run all tests
npm test
```
### 4.项目目录
4-1.项目结构：<br/>
![avatar](/images/webApp.png)<br/>
4-2.src文件结构：<br/>
![avatar](/images/webApp-src.png)<br/>
4-3.page文件结构：<br/>
```file
|-- pages
    |-- applicationDetails
    |   |-- applicationDetails.vue		//应用详情
    |-- common
    |   |-- apply_info.vue
    |   |-- apply_list_item.vue
    |   |-- my_navbar.vue
    |-- hydrant
    |   |-- hydrantInfo.vue
    |   |-- hydrantLists.vue
    |-- notice
        |-- baseInfo.js
        |-- electricalarm_info.vue
        |-- faultalarm_info.vue
        |-- firealarm_info.vue
        |-- firealarm_info_res.vue
        |-- firecock_info.vue
        |-- noticeHistory.vue			//通知二级列表
        |-- noticeInfo.vue
        |-- wateralarm_info.vue

```

### 5.使用技术
>项目使用框架是vue，响应式UI组件和数据，第三方依赖和插件有：vuex、axios、jquery、vue-router、样式采用less预编译.


