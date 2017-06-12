项目所用主要框架： 	 
	AngularJS v1.5.7
		核心思想: 
		MVC、数据绑定、双向数据绑定、模块化
	Bootstrap v3.3.6
	Echarts v3.2.2
	NgFileUpload 12.0.4

图标库:
	Ionicons v2.0.0
	FontAwesome v4.6.3

项目使用技术: 		
	H5、JS、CSS
	Sass: 动态CSS语言
	Bower: 依赖库管理工具
	Gulp: 前端自动化工具
	NodeJS: 本地服务器
	Sublime、Webstorm、Git

项目结构:
	入口文件: 		index.html
	SASS入口: 		public/Andon.scss 	
		公共变量:			public/scss_helper/_variables.scss
		公共Mixin:		public/scss_helper/_mixins.scss
		公共Class:		public/scss_helper/_style.scss
	JS入口:			public/AndonApp.js
	配置文件:			public/AndonConfig.json
	依赖库:			lib/*
	图片存放:			images/*
	业务模块代码:  	modules/*
		modules/alert 				原因状态与报警信息管理模块
			./import 					报警信息导入子模块
			./reasonCode/currentEffect  列表子模块
			./reasonCode/historyDatas   历史记录子模块	

		modules/homePage 			项目首页主模块
			modules/public/andon_footer_bar.html   首页页尾
			modules/public/andon_header_bar.html   首页页眉
			modules/public/andon_footer_bar.html   首页左侧栏

		modules/login 				登录模块

		modules/outputTarget 		产出目标管理模块 [WEB008]

		modules/plannedStop 		停机计划管理模块 [WEB010]

		modules/plc 				PLC管理模块 [WEB005]
			./dataItem 					PLC数据项管理子模块 
			./dataType 					PLC数据类型管理子模块
			./protocolType 				PLC协议类型管理子模块
			./deviceMaintain 			PLC地址管理子模块
				./network 				上位机网络维护子模块 [WEB044]
				./addInfoPart 			PLC地址批量维护子模块
				./addressInfo 			PLC地址信息维护子模块

		modules/reasonCode 			原因代码管理模块  [WEB006]
			./identifyRulesManager   		原因代码规则管理子模块
				./model/create 				原因代码规则新建子模块
				./model/m_detail_info_rule  原因代码规则详情子模块
			./reasonCodeAndDeviceStatus 	原因代码与设备状态管理子模块
			./ReasonCodeGroup 				原因代码组管理模块
			./ReasonCodeMaintain 			原因代码管理模块

		modules/theoryPPM 			PPM管理模块 [WEB007]
			./groupMaintain 			PPM批量维护子模块
			./queryManager 				PPM管理子模块

		modules/user 				权限管理模块
			./userManager 			用户管理子模块
			./userGroupManager      用户组管理子模块
				./basicInfo 				基础信息子模块
				./privilege 				分配子模块
				./platformsPrivilege        子模块

		modules/user/factoryManager 工厂管理模块 [WEB046]

		modules/work 				作业管理模块 [WEB]
			./basiWork 					基本信息管理子模块
			./clientWorkManager 		客户端管理子模块
			./workGroupManager 			作业组管理子模块
			./workParamDesign 			作业组参数管理子模块

		modules/yieldMaintain 		产量维护模块 [WEB009]

		modules/public 				公共模块

Andon项目前端开发规范:
	Andon.scss: 	
			用于improt所有module的主scss文件
	AndonApp.js:	
			用于定义所有module的module变量，eg: plcModule
			用于定义AppModule来启动AngularJS框架
			用于定义全局filter
			用于执行 angularModule.run、angularModule.config等全局依赖
			用于定义BodyCtrl，该controller为项目所有界面controller的父controller
					依据js的原型继承模块，可在该模块中定义其他模块公用的部分, 
						eg:获取权限、初始化左侧栏、提示信息、首页页眉的时间时实更新、编辑状态下页面跳转提示
			用于定义其他模块公共的轻量级变量(重量级变量请放在service中定义)

	AndonConfig.json:
				用于定义项目基本信息，eg:服务器访问地址、版本号等信息

	modules/utils/utilsServices.js
			UtilsService:
				定义整个项目公用方法:
					isEmptyString
					covertUTF8ToZhcn
					isContainZhcn
					confirm
					toUpperCaseWord
					serverFommateDate
					serverFommateDateTime
					....

			HttpAppService:
				定义整个项目Http相关公用方法:
					post
					get
					URLS
					getSite
					getDescByExceptionCode

	SCSS: 每个Module有一个公共的SCSS，用户编写模块公共scss、导入模块其他scss文件
				导入部分要放在最上面（css优先级考虑）

	Module: 
		每个模块定义一个 xxxMoudle.js文件，该文件主要实现以下功能：
			xxxModule.run: 	用户执行该模块启动必需代码:eg:加载该模块相关模板文件到AngularJS模块缓存队列
			xxxService: 	用户定义该模块共用方法，以及该模块所有与http网络请求相关的方法定义
								http相关方法定义规范:
									get请求: 参数少于5个时可以将参数今次传入，超过5个时建议使用对象传入
									post请求: 建议使用对象传入所需参数


以xxx模块为例:（xxx模块为简单模块）
	modules/xxxDir
	modules/xxxDir/xxxModule.js 	
	modules/xxxDir/_xxx.scss
	modules/xxxDir/xxxCtrl.js
	modules/xxxDir/xxx.html
	在index.html文件中引入js文件
	在public/Andon.scss文件中import _xxx.scss文件
	在public/AndonApp.js文件中定义XXX模块路由


以xxx模块为例:（xxx模块为较复杂模块,包含AAA、BBB两个子模块）
	modules/xxxDir
	modules/xxxDir/xxxModule.js 
		xxxService:		xxx模块公共方法、xxx模块所有http接口调用方法	
	modules/xxxDir/_xxx.scss   (在public/Andon.scss中引入该scss文件即可)
		@import "aaa/aaa";
		@import "bbb/bbb";
	modules/xxxDir/xxx.html	

	modules/xxxDir/aaaDir
	modules/xxxDir/aaaDir/_aaa.scss
	modules/xxxDir/aaaDir/aaa.html
	modules/xxxDir/aaaDir/aaaCtrl.js

	modules/xxxDir/bbbDir
	modules/xxxDir/bbbDir/_bbb.scss
	modules/xxxDir/bbbDir/bbb.html
	modules/xxxDir/bbbDir/bbbCtrl.js

	在index.html文件中引入所有js文件
	在public/Andon.scss文件中import _xxx.scss文件
	在public/AndonApp.js文件中定义AAA、BBB子模块路由


Gulp Task:
	watch-sass
	clean
	patch-js
	copy-html
	copy-minify-js-css
	copy-libs
	copy-img
	copy-all
	build-all
	default

