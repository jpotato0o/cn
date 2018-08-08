# 产品概述


网络负载均衡NLB是京东云提供的专注四层业务服务、支持千万级并发连接的高性能负载均衡产品。NLB可自动弹性伸缩、免除用户业务规划与手动升级的繁琐；NLB支持跨多可用区创建、并且与高可用组AG紧密配合，满足业务高可用部署需求；NLB计费除了最小资源保有费之外，完全按照用户实际使用流量进行计费，按用付费为用户提供最经济实用的服务使用方式。


网络负载均衡NLB具备以下功能及特点：

* 高性能：支持千万级并发连接、万级新建连接。
* 高可靠：支持多可用区创建，后端服务可紧密配合高可用组AG的自动跨AZ、跨机架的容错域分配，实现业务自动高可用部署。
* 自动弹性伸缩：服务实例可根据业务情况自动调整，免除用户业务规划与手动升级的繁琐。
* 会话保持：无论NLB还是后端服务的实例扩展时，NLB都能完美实现会话保持能力。
* 连接耗尽：网络负载均衡NLB支持注册实例按照用户定义的连接耗尽条件进行优雅退出，以减少用户业务的中断。
* 后端丰富性：支持虚机、原生容器作为后端服务实例，为用户业务部署提供灵活选择。
* 源地址透传：NLB可完整透传用户源IP地址，便于服务端感知或者统计真实源端信息。
* 按用量计费：NLB计费除了最小资源保有费之外，完全按照用户实际使用流量进行计费，按用付费为用户提供最经济实用的服务使用方式。

## 相关参考

- [产品概述](../Introduction/Overview.md)
- [核心概念](../Introduction/Core-Concepts.md)
- [产品优势](../Product-Introduction/Benefits.md)
- [产品功能](../Product-Introduction/Functions.md)
- [价格总览](../Pricing/Price-Overview.md)
- [计费规则](../Pricing/Billing-Rules.md)
- [创建网络负载均衡实例](../Getting-Started/Create-Instance.md)
- [创建高可用组](../Getting-Started/Create-AvailabilityGroup.md)
- [创建虚拟服务器组](../Operation-Guide/TargetGroup-Management.md)
- [配置侦听策略](../Operation-Guide/Listener-Management.md)
- [管理后端服务与查看服务实例健康状态](../Operation-Guide/Backend-Management.md)
- [查看监控信息](../Operation-Guide/Monitoring.md)