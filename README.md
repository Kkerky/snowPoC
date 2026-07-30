下面按图片顺序逐行翻译。

**图片 1：你发给 Adz 的提问**
Hi Adz,  
你好 Adz，

I would like to ask for your advice on how to collect and manage electronic certificate information.  
我想请教一下，应该如何收集和管理电子证书信息。

The candidate options we are currently considering are MECM and ServiceNow, including ACC.  
我们目前考虑的候选方案是 MECM 和 ServiceNow，包括 ACC。

We would like to confirm whether these options can collect certificate metadata from endpoints and servers,  
我们想确认这些方案是否可以从终端和服务器收集证书元数据，

and whether the collected data can be used for asset management or CI management.  
以及收集到的数据是否可以用于资产管理或 CI 管理。

Specifically, we would like to confirm:  
具体来说，我们想确认以下几点：

Whether MECM standard Hardware Inventory can collect certificate information, or whether PowerShell / WMI extension would be required  
MECM 标准的 Hardware Inventory 是否可以收集证书信息，或者是否需要通过 PowerShell / WMI 扩展来实现。

Whether ServiceNow Discovery, ACC, and/or CMDB can collect and manage this information  
ServiceNow Discovery、ACC 以及/或者 CMDB 是否可以收集并管理这些信息。

Which metadata fields can be collected, such as Subject, Issuer, Thumbprint, Serial Number, validity period, SAN, EKU, certificate store location, etc.  
可以收集哪些元数据字段，例如 Subject、Issuer、Thumbprint、Serial Number、有效期、SAN、EKU、证书存储位置等。

Whether expiry monitoring, notification, and reporting can be implemented  
是否可以实现到期监控、通知和报表。

The scope is limited to certificate metadata only.  
范围仅限于证书元数据。

We do not intend to collect private keys or the certificate files themselves.  
我们不打算收集私钥，也不打算收集证书文件本身。

Any insights would be appreciated.  
如果您能提供任何建议，我们将非常感谢。

Thanks, Kei  
谢谢，Kei。

**图片 2：Adz 的回复**
Hi Kei-san,  
Kei 先生，你好，

The simple answer is that we can do that with SSL/TLS certificate Discovery (ITOM Discovery)  
简单来说，我们可以通过 SSL/TLS certificate Discovery，也就是 ITOM Discovery 来实现。

or direct API calls to the certificate authority for Digicert/Entrust/GoDaddy/Sectigo.  
或者也可以通过直接调用证书颁发机构的 API 来实现，例如 DigiCert、Entrust、GoDaddy、Sectigo。

The standard behaviour will discover the following data:  
标准功能会发现/采集以下数据：

certificate id, revocation_status, subject, issuer, sans/, is_self_signed, is_ca, valid_from, valid_to,  
certificate id、吊销状态、Subject、Issuer、SAN、是否自签名、是否 CA、有效开始日期、有效结束日期，

signature_algorithm, fingerprint_algorithm, key_size, serial_number, and version  
签名算法、指纹算法、密钥长度、序列号和版本。

Expiry monitoring / notifications / reporting can all be implemented,  
到期监控、通知、报表都可以实现，

and even automated renewal workflows can be implemented in some circumstances.  
在某些情况下，甚至可以实现自动续期流程。

While I typically wouldn’t see certificates falling under Asset Management,  
不过通常我不会把证书归类到 Asset Management 下面，

they definitely are used in Configuration Management.  
但它们确实会用于 Configuration Management，也就是配置管理。

The ServiceNow product is “Certificate Inventory and Management”,  
ServiceNow 对应的产品是 “Certificate Inventory and Management”。

the documentation on it is here: Certificate Inventory and Management ... Docs | ServiceNow  
相关文档链接在这里：Certificate Inventory and Management 的 ServiceNow Docs。

Agent Client Collector can be configured to run the same Discovery patterns as well  
Agent Client Collector 也可以配置为运行相同的 Discovery Pattern。

Run Certificate Discovery via ACC-V ... Docs | ServiceNow  
也就是通过 ACC-V 执行 Certificate Discovery，相关文档链接在这里。

**图片 2 下方 / 图片 3：Adz 另一个回复**
Michael Dul I just realised you’re already in this group.  
Michael Dul，我刚意识到你已经在这个群里了。

We have some questions that I’d like to get your thoughts on when you have a moment  
我们有几个问题，想在你有空的时候听听你的意见。

1. The team is working on creating a sandbox environment in Azure where they can spin up devices to test discovery against.  
1. 团队正在 Azure 中创建一个沙箱环境，他们可以在里面启动一些设备，用来测试 Discovery。

Assumption is that once that’s ready, we can get Discovery up and running in the dev instance of ServiceNow to target it.  
目前的假设是，一旦这个环境准备好，我们就可以在 ServiceNow 的开发实例中启动 Discovery，并把它作为目标进行扫描。

Any concerns?  
你觉得这个方案有什么问题或风险吗？

2. Service Mapping.  
2. 关于 Service Mapping。

My thinking is that Service Mapping is a roadmap item for after phase 2 and is not targeted for January 2027.  
我的理解是，Service Mapping 是 Phase 2 之后的路线图项目，并不是以 2027 年 1 月为目标。

Correct or incorrect?  
这个理解对还是不对？

3. Credential Management - current working assumption is that we will store credentials in ServiceNow rather than an external credential store like CyberArk,  
3. 关于 Credential Management，目前的工作假设是：我们会把凭据信息存储在 ServiceNow 中，而不是存储在 CyberArk 这类外部凭据库中。

and the team have not heard any requirement for external credential storage but will check internally with ITSD.  
团队目前还没有听说有必须使用外部凭据库的要求，不过会和 ITSD 内部确认。

Have you heard anything that might suggest a challenge to that assumption?  
你有没有听到什么信息，可能会推翻或挑战这个假设？

**图片 3 / 图片 4：Adz 对山本的确认问题**
Hi 山本拓弥 / YAMAMOTO, TAKUYA-san I have a clarification question  
山本拓弥先生，你好，我有一个想确认的问题。

[GAM] ITOM Cross-Team Alignment.pptx  
关于这个 `[GAM] ITOM Cross-Team Alignment.pptx` 文件。

On slide 5 here we have the test environment layout showing the Yokohama DC and the Azure environment.  
第 5 页里有测试环境的布局图，显示了 Yokohama DC 和 Azure 环境。

Is the DC and Azure shared across many Hitachi companies,  
这里的 DC 和 Azure 是多个 Hitachi 公司共同使用的吗？

## 调查目的

Leader 希望确认：

> Discovery 获取的数据在无法匹配现有 CI 时，能否通过 IRE（Identification and Reconciliation Engine）完成识别、去重，并将 unmatched CI 写入现有自定义表 `u_gam_unmatched_ci`。

## 调查结论

### 最终判定

**IRE 标准功能不能直接完成该需求，但通过“IRE 判断 + 独立写入处理”的组合方式可以实现。**

现有 `u_gam_unmatched_ci` 具有以下属性：

- Scope：`Global`
- 类型：普通自定义 non-CMDB 表
- 未继承 `cmdb_ci`
- 不是 ServiceNow 基础系统预设的 non-CMDB 表

根据 ServiceNow Australia Release 官方说明：

- Application Scope 中的 non-CMDB 表可以获得 IRE 支持。
- Global Scope 中只有 ServiceNow 基础系统预设的部分 non-CMDB 表受到支持。
- 任意创建的 Global 自定义表不属于官方记载的直接支持范围。

因此，**不能将 `u_gam_unmatched_ci` 直接指定为 IRE 的目标表，由 IRE 标准功能对该表完成识别、去重和写入。**

### 可实现的方式

整体业务需求仍然可以实现：

1. 使用 IRE 对 Discovery 数据和正式 CMDB CI 进行识别及去重判断。
2. 判断该数据是否能够匹配现有 CI。
3. 对业务上判定为 unmatched 的数据，使用单独的处理逻辑写入 `u_gam_unmatched_ci`。
4. 该处理可以通过 Script Include、Scheduled Job、Flow、IntegrationHub ETL 或其他定制处理实现。

需要特别说明：

> IRE 在未找到现有 CI 时，通常会根据 payload 的 `className` 在原目标 CI Class 中执行 `INSERT`。IRE 不会自动把该数据转送到另一个任意的自定义表。

因此，本方案属于：

**IRE 负责识别和去重判断，定制逻辑负责向 `u_gam_unmatched_ci` 写入。**

并不是由 IRE 单独完成全部处理。

## 汇报用一句话

> 经调查，现有 `u_gam_unmatched_ci` 是 Global Scope 下未继承 `cmdb_ci` 的普通自定义表，不属于 Australia Release 官方记载的 IRE 直接支持对象，因此不能仅通过 IRE 标准功能完成该表的识别、去重和写入；但可以使用 IRE 判断 Discovery 数据是否匹配正式 CI，再由独立的路由及写入处理将 unmatched 数据登记到该表，从而实现整体业务需求。

## 判断依据

| 判断内容 | 证据等级 | 依据 |
|---|---|---|
| Global Scope 仅支持基础系统预设的部分 non-CMDB 表 | 官方明确记载 | IRE support for non-CMDB tables |
| `u_gam_unmatched_ci` 是 Global、无父表的普通自定义表 | 实例确认 | 当前实例的 Table Definition / XML |
| 该表不属于 IRE 直接支持范围 | 基于官方资料的判断 | 表定义与 Australia 官方支持范围对照 |
| IRE 不会自动将 unmatched 数据转送到任意自定义表 | 基于官方功能定义的判断 | IRE 根据 payload 的目标 Class 执行识别及 INSERT/UPDATE |
| 通过 IRE 判断后由独立处理写入可以实现需求 | 技术方案判断 | IRE API 与独立写入处理组合 |

## 官方参考资料

1. [IRE support for non-CMDB tables](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire-support-non-cmdb-tables.html)  
   用于确认 Australia Release 对 Global/Application Scope 中 non-CMDB 表的支持范围。

2. [Identification and Reconciliation Engine](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire.html)  
   用于确认 IRE 的识别、去重和协调更新机制。

3. [IdentificationEngineScriptableApi](https://www.servicenow.com/docs/r/api-reference/server-api-reference/c_IdentEngineScriptAPI.html)  
   用于确认脚本方式调用 IRE 的能力。

4. [Identification and Reconciliation REST API](https://www.servicenow.com/docs/r/api-reference/rest-apis/c_IdentifyReconcileAPI.html)  
   用于确认通过 REST API 提交和判断 IRE payload 的方式。

5. [Identification simulation](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/identification-simulation.html)  
   用于确认正式写入前进行识别模拟和结果确认的方法。

6. [Create an identification rule for a non-CMDB table](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/create-non-cmdb-id-rule.html)  
   用于确认 non-CMDB Identification Rule 的适用条件和设置方式。

适用版本：ServiceNow Australia Release  
确认日期：2026年7月30日

## 調査目的

Discoveryで取得したデータのうち、CMDBテーブルに正常に保存できなかったデータ（本調査における「unmatched CI」）について、IRE（Identification and Reconciliation Engine）を使用して識別・重複排除を行い、既存のカスタムテーブル u_gam_unmatched_ci に登録できるかを確認する。
## 調査結論

### 最終判定

**IREの標準機能だけで本要件を直接実現することはできません。ただし、「IREによる判定」と「別途用意する登録処理」を組み合わせることで、業務要件全体を実現することは可能です。**

既存の `u_gam_unmatched_ci` テーブルには、以下の特徴があります。

- Scope：`Global`
- テーブル種別：通常のカスタムnon-CMDBテーブル
- `cmdb_ci` を継承していない
- ServiceNowのBase Systemにあらかじめ用意されたnon-CMDBテーブルではない

ServiceNow Australia Releaseの公式ドキュメントには、以下の内容が記載されています。

- Application Scopeでは、non-CMDBテーブルがIREのサポート対象になります。
- Global Scopeでは、Base Systemにあらかじめ用意された一部のnon-CMDBテーブルだけがサポートされます。
- Global Scopeに任意に作成されたカスタムテーブルは、公式ドキュメントに記載された直接サポート範囲には含まれません。

したがって、**`u_gam_unmatched_ci` をIREの対象テーブルとして直接指定し、IREの標準機能だけで識別、重複排除およびレコード登録を行うことはできません。**

### 実現可能な方式

業務要件全体については、以下の構成で実現可能です。

1. IREを使用して、Discoveryデータと正式なCMDB CIの識別および重複判定を行う。
2. Discoveryデータが既存CIと一致するかを判定する。
3. 業務上unmatchedと判定されたデータを、別途用意した処理によって `u_gam_unmatched_ci` に登録する。
4. 登録処理には、Script Include、Scheduled Job、Flow、IntegrationHub ETLなどを使用する。

以下の点には注意が必要です。

> IREが既存CIを特定できなかった場合、通常はpayloadに指定された `className` の対象CIクラスに対して`INSERT`を実行します。IREが自動的に別の任意のカスタムテーブルへデータを転送するわけではありません。

そのため、本要件の実現方式は次のように整理できます。

**IRE：正式なCMDB CIに対する識別・重複判定を担当**

**カスタム処理：unmatchedデータの判定結果を受け取り、`u_gam_unmatched_ci` への登録を担当**

つまり、IRE単独で実現する方式ではなく、**「IRE＋ルーティング／登録処理」**による組み合わせ方式です。

## 報告用の要約

> 調査の結果、既存の `u_gam_unmatched_ci` は、Global Scopeに作成され、`cmdb_ci` を継承していない通常のカスタムnon-CMDBテーブルであることを確認しました。Australia Releaseの公式ドキュメントによると、Global ScopeではBase Systemにあらかじめ用意された一部のnon-CMDBテーブルのみがIREの直接サポート対象です。そのため、IREの標準機能だけで当該テーブルに対する識別、重複排除および登録を行うことはできません。ただし、IREを使用してDiscoveryデータと正式なCMDB CIの一致判定を行い、unmatchedと判定されたデータを別途用意したルーティング／登録処理で当該テーブルへ登録することにより、業務要件全体を実現することは可能です。

## 判断根拠

| 判断内容 | エビデンス区分 | 根拠 |
|---|---|---|
| Global Scopeでは、Base Systemにあらかじめ用意された一部のnon-CMDBテーブルのみがサポートされる | 公式に明記 | IRE support for non-CMDB tables |
| `u_gam_unmatched_ci` はGlobal Scopeで、親テーブルを持たない通常のカスタムテーブルである | インスタンス確認 | 対象インスタンスのTable DefinitionおよびXML |
| `u_gam_unmatched_ci` はIREの直接サポート範囲外である | 公式資料に基づく判断 | テーブル定義とAustralia Releaseの公式サポート範囲を比較 |
| IREはunmatchedデータを任意の別テーブルへ自動転送しない | 公式機能仕様に基づく判断 | IREはpayloadの対象クラスに対して識別および`INSERT`／`UPDATE`を実行する |
| IREの判定後に別処理で登録することで業務要件を実現できる | 技術方式としての判断 | IRE APIとカスタム登録処理の組み合わせ |

## 公式参考資料

1. [IRE support for non-CMDB tables](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire-support-non-cmdb-tables.html)  
   Australia ReleaseにおけるApplication ScopeおよびGlobal Scopeのnon-CMDBテーブルに対するIREサポート範囲の確認に使用。

2. [Identification and Reconciliation Engine](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire.html)  
   IREの識別、重複判定およびReconciliationの基本動作の確認に使用。

3. [IdentificationEngineScriptableApi](https://www.servicenow.com/docs/r/api-reference/server-api-reference/c_IdentEngineScriptAPI.html)  
   スクリプトからIREを呼び出す方法の確認に使用。

4. [Identification and Reconciliation REST API](https://www.servicenow.com/docs/r/api-reference/rest-apis/c_IdentifyReconcileAPI.html)  
   REST APIを使用してIRE payloadを送信する方法の確認に使用。

5. [Identification simulation](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/identification-simulation.html)  
   データを登録する前にIdentification結果をシミュレーションする方法の確認に使用。

6. [Create an identification rule for a non-CMDB table](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/create-non-cmdb-id-rule.html)  
   non-CMDBテーブルに対するIdentification Ruleの適用条件と設定方法の確認に使用。

対象バージョン：ServiceNow Australia Release  
確認日：2026年7月30日
-----------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------- 
## 調査目的

Leaderからの調査依頼は、以下の内容です。

> Discoveryで取得したデータのうち、CMDBテーブルに正常に保存できなかったデータについて、IRE（Identification and Reconciliation Engine）を使用して識別・重複排除を行い、既存のカスタムテーブル `u_gam_unmatched_ci` に登録できるかを確認する。

本調査では、上記のデータを便宜上「unmatched CI」と呼びます。

ここでいう「unmatched CI」とは、単に既存CIとのIdentificationに失敗したデータではなく、Discoveryでは情報を取得できたものの、何らかの理由によりCMDBテーブルへ正常に登録できなかったデータを指します。

## 調査結論

### 最終判定

**現在の `u_gam_unmatched_ci` のテーブル定義を変更しない前提では、IREの標準機能だけで本要件を実現することはできません。**

ただし、DiscoveryまたはIREの処理結果からCMDBへ保存できなかったデータを取得し、別途用意した処理によって識別・重複排除および当該テーブルへの登録を行うことで、業務要件全体を実現することは可能です。

### 判断理由

対象の `u_gam_unmatched_ci` テーブルには、以下の特徴があります。

- Scope：`Global`
- 通常のカスタムnon-CMDBテーブル
- `cmdb_ci` を継承していない
- ServiceNowのBase Systemにあらかじめ用意されたテーブルではない

ServiceNow Australia Releaseでは、Global Scopeのnon-CMDBテーブルについて、Base Systemにあらかじめ用意された一部のテーブルのみがIREのサポート対象です。

そのため、`u_gam_unmatched_ci` をIRE payloadの対象テーブルとして指定し、IREに識別、重複排除およびレコード登録を直接実行させることは、公式に記載された標準サポート範囲外です。

## CMDBに保存できなかったデータの扱い

CMDBへ保存できなかったDiscoveryデータは、すべて同じ状態になるわけではありません。

### IREまで到達したデータ

Enhanced IREの処理対象になったデータは、エラー内容によって次の標準テーブルに保存される場合があります。

- Partial Payload：`cmdb_ire_partial_payloads`
- Incomplete Payload：`cmdb_ire_incomplete_payloads`

Partial Payloadは、後続データによって不足情報が補完された場合、再処理される可能性があります。

Incomplete Payloadは、復旧不能なエラーを含むpayloadの記録を目的として保存され、再処理されません。

IREがこれらのデータを `u_gam_unmatched_ci` へ自動転送する標準機能はありません。

### IREまで到達していないデータ

DiscoveryのPattern、Classification、Credential、通信またはその他の処理で失敗し、IREへpayloadが送信されていない場合、そのデータをIREで識別・重複排除することはできません。

したがって、対象データがDiscovery処理のどの段階で保存できなかったかを確認する必要があります。

## 実現可能な構成

業務要件を実現する場合は、以下のような独立した処理が必要です。

1. Discovery Status、Discovery Log、IRE処理結果、Partial Payload、Incomplete Payloadなどから対象データを取得する。
2. CMDBへ保存できなかった原因を分類する。
3. `u_gam_unmatched_ci` 用の識別キーを使用して重複を判定する。
4. 未登録の場合は新規登録し、登録済みの場合は更新または処理をスキップする。
5. 元データ、Discovery実行情報、エラー理由、処理日時などを記録する。

IREまで到達したデータについては、IREの処理結果を判定材料として利用できる場合があります。

ただし、`u_gam_unmatched_ci` 自体に対する識別・重複排除および登録は、IREの標準機能ではなく、別途用意するカスタム処理が担当します。

## 報告用の要約

> 調査の結果、Discoveryで取得したもののCMDBテーブルへ正常に保存できなかったデータを、IREの標準機能だけで既存の `u_gam_unmatched_ci` に識別・重複排除して登録することはできないと判断しました。当該テーブルはGlobal Scopeに作成された通常のカスタムnon-CMDBテーブルであり、Australia ReleaseにおけるIREの直接サポート範囲外です。また、CMDBへの保存失敗がIRE処理前に発生した場合、IRE自体がそのデータを処理することもできません。一方、DiscoveryおよびIREの処理結果から対象データを取得し、別途用意した処理で識別・重複排除したうえで `u_gam_unmatched_ci` に登録する方式であれば、業務要件を実現することは可能です。

## 判定一覧

| 確認項目 | 判定 |
|---|---|
| IREから `u_gam_unmatched_ci` へ直接登録する | 不可（標準サポート範囲外） |
| IREですべてのCMDB保存失敗データを識別する | 不可（IREへ到達していないデータは処理できない） |
| IREのエラー／処理結果を判定材料として利用する | 条件付きで可能 |
| 独立した処理で失敗データを収集する | 可能 |
| 独立した処理で重複排除して当該テーブルへ登録する | 可能 |
| IRE単独で本要件全体を実現する | 不可 |
| IRE結果とカスタム処理を組み合わせて実現する | 可能 |

## 公式参考資料

1. [IRE support for non-CMDB tables](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire-support-non-cmdb-tables.html)  
   Global Scopeにおけるnon-CMDBテーブルのIREサポート範囲の判断根拠。

2. [Identification and Reconciliation Engine](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire.html)  
   IREの処理、Partial Payload、Incomplete Payloadおよび各標準保存先の確認根拠。

対象バージョン：ServiceNow Australia Release  
確認日：2026年7月30日
or do different companies have their own datacentre sites and Azure accounts?  
还是说不同公司各自拥有自己的数据中心站点和 Azure 账号？

-----------------------------------------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------- 
## 调查目的

Leader希望确认以下内容：

> 对于Discovery已经取得、但无法正常保存到CMDB表中的数据，是否可以使用IRE（Identification and Reconciliation Engine）进行识别和去重，并将其登记到现有自定义表 `u_gam_unmatched_ci` 中。

本调查暂时将上述数据称为“unmatched CI”。

这里的“unmatched CI”不只是指无法与现有CI匹配的数据，而是泛指Discovery已经取得相关信息，但由于某种原因未能正常登记到CMDB表中的数据。

## 调查结论

### 最终判定

**在不修改现有 `u_gam_unmatched_ci` 表定义的前提下，无法仅通过IRE标准功能直接实现该需求。**

但是，可以从Discovery或IRE的处理结果中取得未能保存到CMDB的数据，再通过独立处理完成识别、去重并写入该自定义表。因此，整体业务需求在技术上可以实现。

### 判断理由

现有 `u_gam_unmatched_ci` 表具有以下属性：

- Scope：`Global`
- 类型：普通自定义non-CMDB表
- 未继承 `cmdb_ci`
- 不是ServiceNow Base System预设表

根据ServiceNow Australia Release官方说明：

- Application Scope中的non-CMDB表属于IRE支持对象。
- Global Scope中只有ServiceNow Base System预设的部分non-CMDB表受到支持。
- 在Global Scope中任意创建的自定义表不属于官方记载的直接支持范围。

因此，不能将 `u_gam_unmatched_ci` 直接指定为IRE payload的目标表，再由IRE对该表执行识别、去重和记录写入。

## CMDB保存失败数据的处理

Discovery取得的数据未能保存到CMDB时，并不一定都会进入相同的处理状态。

### 已经进入IRE的数据

对于已经进入Enhanced IRE处理的数据，根据错误内容，可能会被保存到以下标准表中：

- Partial Payload：`cmdb_ire_partial_payloads`
- Incomplete Payload：`cmdb_ire_incomplete_payloads`

Partial Payload表示数据暂时不完整。当后续数据补全缺失信息时，IRE可能重新处理这些数据。

Incomplete Payload用于保存包含不可恢复错误的payload，主要用于错误记录，之后不会被重新处理。

IRE没有将这些数据自动转送到 `u_gam_unmatched_ci` 的标准功能。

### 尚未进入IRE的数据

如果Discovery在以下阶段发生错误，相关数据可能尚未发送给IRE：

- Pattern执行
- Classification
- Credential认证
- MID Server通信
- Discovery其他前置处理

对于没有到达IRE的数据，IRE无法执行识别和去重。

因此，在设计具体处理方式之前，需要先确认数据是在Discovery处理的哪个阶段未能保存到CMDB。

## 可以实现的处理结构

要实现整体业务需求，需要增加以下独立处理：

1. 从Discovery Status、Discovery Log、IRE处理结果、Partial Payload、Incomplete Payload等位置取得目标数据。
2. 判断数据未能保存到CMDB的具体原因。
3. 根据 `u_gam_unmatched_ci` 使用的识别键进行重复判断。
4. 如果不存在相同记录，则新建记录。
5. 如果已经存在相同记录，则更新现有记录或跳过处理。
6. 保存原始数据、Discovery执行信息、错误原因和处理时间等审计信息。

对于已经到达IRE的数据，可以根据实际情况，将IRE的处理结果作为判断依据。

但是，对 `u_gam_unmatched_ci` 本身执行识别、去重和写入的部分，需要由独立的自定义处理负责，不能由IRE标准功能直接完成。

## 汇报用总结

> 经调查，对于Discovery已经取得、但未能正常保存到CMDB表中的数据，无法仅通过IRE标准功能完成识别、去重并直接登记到现有的 `u_gam_unmatched_ci` 表中。该表属于Global Scope下的普通自定义non-CMDB表，不在ServiceNow Australia Release官方记载的IRE直接支持范围内。另外，如果保存失败发生在数据进入IRE之前，IRE也无法处理该数据。但是，通过Discovery及IRE的处理结果取得目标数据，再由独立处理执行识别、去重并写入 `u_gam_unmatched_ci`，则可以实现整体业务需求。

## 判定一览

| 确认项目 | 判定 |
|---|---|
| 由IRE直接写入 `u_gam_unmatched_ci` | 不可，超出标准支持范围 |
| 使用IRE识别所有CMDB保存失败数据 | 不可，IRE无法处理尚未到达IRE的数据 |
| 使用IRE错误及处理结果作为判断依据 | 有条件可行 |
| 通过独立处理收集保存失败数据 | 可行 |
| 通过独立处理去重并写入该自定义表 | 可行 |
| 仅使用IRE实现全部需求 | 不可 |
| 结合IRE结果与自定义处理实现需求 | 可行 |

## 判断依据

| 判断内容 | 证据等级 | 依据 |
|---|---|---|
| Global Scope仅支持Base System预设的部分non-CMDB表 | 官方明确记载 | IRE support for non-CMDB tables |
| `u_gam_unmatched_ci` 是Global Scope、无父表的普通自定义表 | 实例确认 | 当前实例的Table Definition及XML |
| 该表不属于IRE直接支持范围 | 基于官方资料的判断 | 表定义与Australia Release官方支持范围对照 |
| Partial Payload可能保存到 `cmdb_ire_partial_payloads` | 官方明确记载 | Identification and Reconciliation Engine |
| Incomplete Payload可能保存到 `cmdb_ire_incomplete_payloads` | 官方明确记载 | Identification and Reconciliation Engine |
| IRE不会自动将数据转送到该自定义表 | 基于官方功能定义的判断 | IRE只处理payload指定的受支持目标表或CI Class |
| 通过独立处理可以实现整体业务需求 | 技术方案判断 | 对Discovery及IRE结果进行收集、判断和独立写入 |

## 官方参考资料

1. [IRE support for non-CMDB tables](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire-support-non-cmdb-tables.html)  
   用于确认Australia Release中Global Scope和Application Scope下non-CMDB表的IRE支持范围。

2. [Identification and Reconciliation Engine](https://www.servicenow.com/docs/r/servicenow-platform/configuration-management-database-cmdb/ire.html)  
   用于确认IRE处理机制、Partial Payload、Incomplete Payload及对应标准保存表。

适用版本：ServiceNow Australia Release  
确认日期：2026年7月30日
