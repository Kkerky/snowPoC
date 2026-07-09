Hi Adz,

I would like to ask for your advice on how to collect and manage electronic certificate information.

The candidate options we are currently considering are MECM and ServiceNow, including ACC. We would like to confirm whether these options can collect certificate metadata from endpoints and servers, and whether the collected data can be used for asset management or CI management.

Specifically, we would like to confirm:

- Whether MECM standard Hardware Inventory can collect certificate information, or whether PowerShell / WMI extension would be required
- Whether ServiceNow Discovery, ACC, and/or CMDB can collect and manage this information
- Which metadata fields can be collected, such as Subject, Issuer, Thumbprint, Serial Number, validity period, SAN, EKU, certificate store location, etc.
- Whether expiry monitoring, notification, and reporting can be implemented

The scope is limited to certificate metadata only. We do not intend to collect private keys or the certificate files themselves.

Best regards,
Kei

想咨询一下电子证书信息的收集和资产管理方式。

目前考虑的候选方式包括 MECM 和 ServiceNow（包含 ACC）。想确认这些方式是否可以收集终端/服务器上的证书元数据，并用于资产管理或 CI 管理。

具体想确认：
MECM 标准 Hardware Inventory 是否可以取得证书信息，还是需要 PowerShell / WMI 扩展；
ServiceNow Discovery / ACC / CMDB 是否可以收集并管理这些信息；
可取得的项目有哪些，例如 Subject、Issuer、Thumbprint、Serial Number、有效期、SAN、EKU、证书存储位置等；
以及是否可以实现到期监视、通知和报表化。

范围仅限证书元数据，不包含私钥或证书文件本体。
ServiceNow Dev环境 PoC插件安装与留痕手册
ITOM Discovery / Service Mapping / ACC 验证准备用
项目	内容
适用对象	公司 ServiceNow Dev 环境中，本次 Azure PoC 所需插件 / 应用的确认、安装、申请与证跡整理。
作业方针	不新建 Application Scope。配置变更使用 PoC 专用 Update Set 管理，插件安装本身通过截图、安装详情、安装履历留痕。
PoC 范围	ITOM1 / ITOM2 / ITOM3 内部 Discovery、Palo Alto / FortiGate 发现、MID Server、ACC、Service Mapping、CSDM 对应关系验证。
作业日期	YYYY/MM/DD
操作者	填写操作者账号 / 氏名

先に確認する結論
本作業では新規 Application Scope は作成しない。通常は Global のまま作業する。
Plugin 導入は Update Set だけでは十分に証跡化できないため、Application Manager の画面と Installation Details を必ず保存する。
一部 Plugin は ServiceNow personnel による有効化、または Now Support 申請が必要になる可能性がある。

0. 基本方针
本手册用于 Dev 环境中安装 PoC 所需 ServiceNow 插件 / 应用，并整理现场可追溯的证跡。目标不是只完成安装，而是让后续审查时能够说明“为什么安装、安装了什么版本、何时由谁安装、安装前后状态如何、是否存在依赖和申请记录”。
判断项	本次方针	说明
Application Scope	不新建	本次不是开发自定义应用，而是安装 ServiceNow 标准插件 / 应用。新建 Scope 反而可能引入 cross-scope 说明成本。
Update Set	新建 PoC 专用 Update Set	用于记录后续配置变更，例如 Discovery Schedule、IP Range、Credential、CSDM 示例数据等。
Plugin 安装证跡	截图 + 安装详情 + 安装后状态	插件安装与依赖关系不应只依赖 Update Set 说明，需要单独保存画面证跡。
Demo Data	默认不安装	除非客户或负责人明确要求，否则不导入 demo data，避免污染 Dev 验证数据。
Discovery 执行	插件安装后暂不立即执行	先完成网络范围、URL Filter、MID Server、Credential、NSG/Firewall 等确认后，再执行 Discovery。

1. 事前准备
No.	确认项目	操作 / 记录内容	证跡
1	Dev 实例信息	记录 Instance URL、Instance Name、Release family / build、作业日期、操作者账号。	实例首页或 System Diagnostics 相关画面截图。
2	权限确认	确认操作者具备 admin 或插件安装 / Application Manager 操作权限。	用户角色画面或操作成功画面截图。
3	作业窗口	确认是否有公司指定作业时间、变更编号、审批编号。	变更票、邮件、Teams 记录等。
4	PoC Update Set	创建并切换到 PoC 专用 Update Set。建议名称：US_ServiceNow_Azure_PoC_ITOM_Config_YYYYMMDD。ServiceNow Azure PoC 用 ITOM Discovery / Service Mapping / ACC / CSDM 関連設定変更を記録する Update Set。
Plugin 導入自体は Application Manager の画面証跡、導入前後状態、Version 情報、導入履歴で別途管理する。
	Local Update Sets 画面和当前 Update Set 显示截图。
5	当前 Scope	确认当前 Application Scope 维持 Global。	页面右上角 Application picker 或系统设定画面截图。
6	证跡目录	创建证跡目录并统一命名。建议：PoC_Plugin_Install_Evidence_YYYYMMDD。	目录结构截图。

证跡目录建议
01_事前確認
02_插件安装前状态
03_安装过程
04_安装后确认
05_NowSupport申请记录
06_异常与补充说明

2. 需要确认的插件 / 应用清单
下表为本次 PoC 的确认候补。实际显示名、ID、版本、依赖关系以 Dev 实例的 Application Manager / Store / Installation Details 为准。若找不到或按钮不可用，不要强行处理，按第 5 章走申请流程。
优先级	显示名 / Plugin ID 候补	用途	本次判断	安装前必须留痕
必须	Discovery / com.snc.discovery	IP Range Discovery、Windows / Linux、Network Device Discovery 的基础。	必须确认并安装。	安装前状态、版本、依赖、是否已安装。
必须	Discovery and Service Mapping Patterns / com.sn_itom_pattern	Discovery / Service Mapping Pattern 内容。	必须确认版本。	Installed version、是否有 update。
必须	Service Mapping / com.snc.service-mapping	Top-down / Pattern-based Mapping、Application Service 相关验证。	5.8 / 5.9 验证需要。	安装状态、依赖、版本。
强建议	Discovery Admin Workspace / sn_disco_workspace	Discovery 管理、状态确认、错误调查。	有则操作和证跡整理更容易。	是否可打开 Workspace。
强建议	Visibility Content	Discovery Admin Workspace 及 ITOM Visibility 相关内容。	按实例显示确认。	版本、依赖关系。
强建议	CMDB Workspace	CMDB / CI / CSDM 结果确认。	用于验证结果查看。	菜单是否出现、是否可访问。
条件必须	Agent Client Collector Framework / sn_agent	ACC Agent 基础框架。	如果做 ACC 验证则必须。	安装状态、版本、依赖。
条件必须	Agent Client Collector for Visibility	ACC Inventory / Visibility 相关验证。	如果做 ACC 取得项验证则必须。	安装状态、配置入口。
确认项	Discovery - IP Based / com.snc.discovery.ip_based	Credential-less Discovery / Nmap 相关。	通常随 Discovery / Service Mapping 相关安装激活，仍需确认。	是否已安装、是否可用。

3. Dev 环境操作手顺
Step	操作位置	具体操作	留痕要求
0-1	证跡目录	创建 PoC_Plugin_Install_Evidence_YYYYMMDD 目录，并创建子目录。
01_事前確認
02_插件安装前状态
03_安装过程
04_安装后确认
05_NowSupport申请记录
06_异常与补充说明
	保存目录结构截图。
0-2	ServiceNow Dev	登录 Dev 实例，记录 URL、账号、Release family / build。
https://xxxxx.service-now.com/stats.do	首页、版本信息、账号信息截图。
0-3	System Update Sets > Local Update Sets	新建 Update Set：US_ServiceNow_Azure_PoC_ITOM_Config_YYYYMMDD。State 保持 In progress。	新建画面和当前 Update Set 截图。
0-4	Application Picker / Scope	确认当前 Application Scope 为 Global。不要新建自定义 Scope。	当前 Scope 截图。
1-1	Admin > Application Manager	打开 Application Manager，切到 Installed / Available for you，逐个搜索插件。
1.	ITOMVisibility
2.	Discovery and Service Mapping Patterns	搜索结果截图。
1-2	Application Manager Details	打开插件详情，确认 Display name、Plugin ID、Version、Dependencies、Installation Details。	详情页完整截图。
1-3	Application Manager	如果显示可安装，点击 Install。Demo data 默认不选择。	安装确认画面截图。
1-4	Installation progress / Activity	等待安装完成，记录安装过程和结果。	Progress / Activity log 截图。
1-5	Installed	安装后再次搜索同一插件，确认状态为 Installed，并记录版本。	安装后状态截图。
1-6	All menu / Workspace	确认 Discovery、Service Mapping、ACC、CMDB Workspace 等菜单是否出现。	菜单搜索结果或 Workspace 首页截图。
1-7	作业记录表	将每个插件的结果填入安装确认表。	保存最终确认表。

4. 安装后确认项目
No.	确认对象	确认内容	OK 条件	证跡
1	Discovery	菜单或 Workspace 是否可访问。	Discovery 相关画面可打开。	菜单 / Workspace 截图。
2	MID Server	MID Server 管理页面是否可访问。	MID Server 列表页面可打开。	列表页面截图。
3	Service Mapping	Service Mapping 菜单和 Application Service 相关入口是否出现。	菜单可见，权限错误なし。	菜单搜索截图。
4	Patterns	Pattern Designer / Pattern 相关入口是否可见。	Pattern 相关画面可打开。	Pattern 入口截图。
5	ACC	ACC Framework / ACC Visibility 相关入口是否出现。	ACC 相关菜单可见。	菜单 / 设置页截图。
6	CMDB / CSDM	CMDB Workspace、CI Class、Business Service / Offering 相关表是否可访问。	验证用数据创建入口可访问。	相关列表页截图。
7	Update Set	确认当前 Update Set 仍为 PoC 专用 Update Set。	当前 Update Set 未错切。	当前 Update Set 截图。

5. 找不到插件 / 无法安装时的申请流程
如果插件搜索不到、Install 按钮不可用、提示 license / entitlement / subscription 不足，或显示需要 ServiceNow personnel 激活，则不要绕过流程。应收集当前画面证跡并提交插件申请。
场景	处理方式	留痕
搜索不到插件	记录搜索关键词、筛选条件、实例 URL，并确认是否使用正确实例和权限。	搜索结果为 0 的截图。
Install 按钮不可用	确认是否缺少 entitlement / subscription / role，并准备 Request Plugin。	按钮灰掉或错误信息截图。
需要 ServiceNow 激活	通过 Now Support / Request Plugin 提交申请，指定 Dev 实例和插件名称。	申请编号、申请内容截图。
依赖安装失败	记录失败插件、失败时间、错误信息、Activity log。	失败日志截图。
安装后菜单不可见	确认角色、缓存、应用权限、插件状态。必要时重新登录或联系管理员。	安装状态和菜单搜索截图。

6. Request Plugin 申请内容模板
项目	填写例
Subject	Request plugin activation for ServiceNow Azure PoC Dev instance
Target instance	Dev instance URL / instance name
Requested plugin	Plugin display name and Plugin ID
Purpose	ServiceNow Azure PoC validation for ITOM Discovery, Service Mapping, ACC, and CSDM relationship verification.
Business reason	PoC requires validation of Discovery for Windows/Linux VM, Palo Alto, FortiGate, ACC inventory, and Service Mapping / CSDM relationship confirmation.
Demo data	Not required unless explicitly approved.
Preferred schedule	YYYY/MM/DD HH:mm - HH:mm JST
Requester	Name / department / email

日文申请说明例
今回の Azure PoC において、ITOM Discovery / Service Mapping / ACC / CSDM 対応関係を検証するため、Dev インスタンスへの対象 Plugin 導入を申請します。
Demo data は不要です。導入対象は Dev 環境に限定し、本番環境への影響はありません。

7. 留痕命名规则
类型	文件名例	内容
事前确认	01_事前確認_DevInstance_YYYYMMDD.png	Dev URL、Release、操作者、当前 Update Set。
插件安装前	02_Pre_Discovery_com.snc.discovery_YYYYMMDD.png	插件安装前状态、版本、依赖。
安装过程	03_Install_Discovery_Progress_YYYYMMDD.png	Install progress、Activity log。
安装后	04_Post_Discovery_Installed_YYYYMMDD.png	Installed 状态、版本、更新状态。
菜单确认	05_Menu_Discovery_Workspace_YYYYMMDD.png	菜单或 Workspace 可访问状态。
申请记录	06_RequestPlugin_ServiceMapping_REQxxxx.png	Now Support / Request Plugin 申请编号和内容。
异常	99_Error_PluginName_YYYYMMDD.png	错误信息、失败日志、补充说明。

8. 插件安装确认表模板
No.	插件 / 应用名	Plugin ID	安装前状态	安装后状态	版本	依赖 / 备注	证跡文件
1	Discovery	com.snc.discovery					
2	Discovery and Service Mapping Patterns	com.sn_itom_pattern					
3	Service Mapping	com.snc.service-mapping					
4	Discovery Admin Workspace	sn_disco_workspace					
5	Agent Client Collector Framework	sn_agent					
6	Agent Client Collector for Visibility	以实例显示为准					

9. 注意事项
No.	注意事项	说明
1	不要随意安装范围外插件	Event Management、Security、Now Assist、Service Graph Connector 等不属于本次 PoC 范围时，不应顺手安装。
2	不要导入 Demo Data	除非明确批准，否则 Demo Data 会影响 Dev 环境数据洁净度。
3	不要立即执行大范围 Discovery	插件安装成功不等于网络和凭据已准备好。Discovery 范围应限定 ITOM1/2/3。
4	Nmap / Credential-less Discovery 需谨慎	云环境和公司网络策略可能限制扫描行为，执行前应确认范围和许可。
5	Update Set 不是全部证跡	插件安装、依赖、安装履历、申请编号仍需截图和记录。
6	安装失败不要反复尝试	先保存错误信息，再确认权限、订阅、依赖和申请流程。

10. 官方参考资料
主题	官方资料
ServiceNow Plugins	https://www.servicenow.com/docs/r/platform-administration/c_ServiceNowPlugins.html
Application Manager 安装应用 / 插件	https://www.servicenow.com/docs/r/platform-administration/application-manager/installing-apps-app-manager.html
Request a plugin	https://www.servicenow.com/docs/r/platform-administration/t_RequestAPlugin.html
Application Scope	https://www.servicenow.com/docs/r/application-development/c_ApplicationScope.html
System Update Sets	https://www.servicenow.com/docs/r/application-development/system-update-sets/system-update-sets.html
ITOM Visibility 插件 / 应用	https://www.servicenow.com/docs/r/it-operations-management/itom-visibility/plugin-app-itom-visibility.html
Credential-less Discovery with Nmap	https://www.servicenow.com/docs/r/it-operations-management/discovery/nmap-credential-less-discovery.html

