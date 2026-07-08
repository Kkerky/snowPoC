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
