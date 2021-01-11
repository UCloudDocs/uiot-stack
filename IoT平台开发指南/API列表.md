# API列表

## 产品管理
* [创建产品](uiot-stack/IoT平台开发指南/产品管理?id=创建产品)   



| [ModifyUIoTCoreProduct](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=modifyuiotcoreproduct) | 修改物联网产品             |
| [DeleteUIoTCoreProduct](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=deleteuiotcoreproduct) | 删除物联网产品             |
| [GetUIoTCoreProductList](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=getuiotcoreproductlist) | 获取产品信息列表           |
| [EnableUIoTCoreDynamicRegister](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=enableuiotcoredynamicregister) | 打开产品下设备动态注册功能 |
| [DisableUIoTCoreDynamicRegister](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=disableuiotcoredynamicregister) | 禁用产品下设备动态注册     |
| [PublishUIoTCoreProduct](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=publishuiotcoreproduct) | 发布产品                   |
| [UnpublishUIoTCoreProduct](https://docs.ucloud.cn/uiot-core/api_guide/productmgmtapi?id=unpublishuiotcoreproduct) | 撤销发布产品               |

## 设备管理

| API名称                                                      | 描述                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| [CreateUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=createuiotcoredevice) | 创建物联网设备                               |
| [GetUIoTCoreDeviceInfo](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=getuiotcoredeviceinfo) | 获取设备信息                                 |
| [ModifyUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=modifyuiotcoredevice) | 修改物联网设备                               |
| [DeleteUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=deleteuiotcoredevice) | 删除设备                                     |
| [EnableUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=enableuiotcoredevice) | 启用设备                                     |
| [DisableUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=disableuiotcoredevice) | 禁用设备                                     |
| [BatchCreateUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=batchcreateuiotcoredevice) | 批量创建物联网设备                           |
| [BatchCreateUIoTCoreDeviceWithSN](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=batchcreateuiotcoredevicewithsn) | 批量创建物联网设备                           |
| [BatchDeleteUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=batchdeleteuiotcoredevice) | 批量删除设备                                 |
| [BatchEnableUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=batchenableuiotcoredevice) | 批量启用设备                                 |
| [BatchDisableUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=batchdisableuiotcoredevice) | 批量禁用设备                                 |
| [ResetUIoTCoreDevice](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=resetuiotcoredevice) | 重置设备激活状态                             |
| [GetUIoTCoreDeviceList](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=getuiotcoredevicelist) | 获取单个或批量设备状态、设备详情、设备列表等 |
| [GetUIoTCoreInactivatedDevicePasswordFile](https://docs.ucloud.cn/uiot-core/api_guide/devicemgmtapi?id=getuiotcoreinactivateddevicepasswordfile) | 获取未激活设备的密码文件                     |



## 主题管理

| API名称                                                      | 描述              |
| ------------------------------------------------------------ | ----------------- |
| [CreateUIoTCoreProductTopic](https://docs.ucloud.cn/uiot-core/api_guide/topicmgmt?id=createuiotcoreproducttopic) | 创建产品Topic     |
| [ModifyUIoTCoreProductTopic](https://docs.ucloud.cn/uiot-core/api_guide/topicmgmt?id=modifyuiotcoreproducttopic) | 修改产品Topic     |
| [DeleteUIoTCoreProductTopic](https://docs.ucloud.cn/uiot-core/api_guide/topicmgmt?id=deleteuiotcoreproducttopic) | 删除产品Topic     |
| [GetUIoTCoreProductTopicList](https://docs.ucloud.cn/uiot-core/api_guide/topicmgmt?id=getuiotcoreproducttopiclist) | 获取产品topic列表 |



## 消息通信

| API名称                                                      | 描述             |
| ------------------------------------------------------------ | ---------------- |
| [PublishUIoTCoreMQTTMessage](https://docs.ucloud.cn/uiot-core/api_guide/messagemgmtapi?id=publishuiotcoremqttmessage) | Publish MQTT消息 |
| [BroadcastUIoTCoreMQTTMessage](https://docs.ucloud.cn/uiot-core/api_guide/messagemgmtapi?id=broadcastuiotcoremqttmessage) | 广播消息         |
| [PublishUIoTCoreMQTTMessage](https://docs.ucloud.cn/uiot-core/api_guide/messagemgmtapi?id=publishuiotcoremqttmessage) | RRPC             |
| [BroadcastUIoTCoreMQTTMessage](https://docs.ucloud.cn/uiot-core/api_guide/messagemgmtapi?id=broadcastuiotcoremqttmessage) | 广播消息         |




## 规则引擎

| API名称                                                      | 描述               |
| ------------------------------------------------------------ | ------------------ |
| [CreateUIoTCoreRule](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=createuiotcorerule) | 创建规则           |
| [GetUIoTCoreRuleList](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=getuiotcorerulelist) | 获取规则列表       |
| [ModifyUIoTCoreRule](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=modifyuiotcorerule) | 修改规则           |
| [DeleteUIoTCoreRule](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=deleteuiotcorerule) | 删除规则           |
| [EnableUIoTCoreRule](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=enableuiotcorerule) | 启用规则           |
| [DisableUIoTCoreRule](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=disableuiotcorerule) | 禁用规则           |
| [CreateUIoTCoreRuleAction](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=createuiotcoreruleaction) | 创建规则Action     |
| [GetUIoTCoreRuleActionList](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=getuiotcoreruleactionlist) | 获取规则Action列表 |
| [ModifyUIoTCoreRuleAction](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=modifyuiotcoreruleaction) | 修改规则Action     |
| [DeleteUIoTCoreRuleAction](https://docs.ucloud.cn/uiot-core/api_guide/ruleeneinmgmt?id=deleteuiotcoreruleaction) | 删除规则Action     |



## 网关及子设备

| API名称                                                      | 描述               |
| ------------------------------------------------------------ | ------------------ |
| [AddUIoTCoreSubDeviceTopo](https://docs.ucloud.cn/uiot-edge/api_list/gateway_subdevice?id=adduiotcoresubdevicetopo) | 添加子设备拓扑     |
| [DeleteUIoTCoreSubDeviceTopo](https://docs.ucloud.cn/uiot-edge/api_list/gateway_subdevice?id=deleteuiotcoresubdevicetopo) | 删除子设备拓扑     |
| [GetUIoTCoreEdgeList](https://docs.ucloud.cn/uiot-edge/api_list/gateway_subdevice?id=getuiotcoreedgelist) | 获取边缘网关列表   |
| [GetUIoTCoreGatewayBySubDevice](https://docs.ucloud.cn/uiot-edge/api_list/gateway_subdevice?id=getuiotcoregatewaybysubdevice) | 获取子设备网关信息 |
| [GetUIoTCoreSubDeviceList](https://docs.ucloud.cn/uiot-edge/api_list/gateway_subdevice?id=getuiotcoresubdevicelist) | 获取子设备列表     |



## 安装及部署

| API名称                                                      | 描述                 |
| ------------------------------------------------------------ | -------------------- |
| [CreateUIoTCoreReinstall](https://docs.ucloud.cn/uiot-edge/api_list/install_deploy?id=createuiotcorereinstall) | 重装软件             |
| [GetUIoTCoreReinstallInfo](https://docs.ucloud.cn/uiot-edge/api_list/install_deploy?id=getuiotcorereinstallinfo) | 获取设备网关安装信息 |
| [CreateUIoTCoreEdgeDeployment](https://docs.ucloud.cn/uiot-edge/api_list/install_deploy?id=createuiotcoreedgedeployment) | 创建边缘部署任务     |
| [GetUIoTCoreEdgeDeploymentInfo](https://docs.ucloud.cn/uiot-edge/api_list/install_deploy?id=getuiotcoreedgedeploymentinfo) | 获取边缘部署任务信息 |
| [GetUIoTCoreEdgeDeploymentList](https://docs.ucloud.cn/uiot-edge/api_list/install_deploy?id=getuiotcoreedgedeploymentlist) | 获取边缘部署任务列表 |



## 子设备驱动与接入

| API名称                                                      | 描述                     |
| ------------------------------------------------------------ | ------------------------ |
| [CreateUIoTCoreDriver](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=createuiotcoredriver) | 创建驱动                 |
| [DeleteUIoTCoreDriver](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=deleteuiotcoredriver) | 删除驱动                 |
| [ModifyUIoTCoreDriver](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=modifyuiotcoredriver) | 修改驱动                 |
| [GetUIoTCoreDriverInfo](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoredriverinfo) | 获取驱动信息             |
| [GetUIoTCoreDriverList](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoredriverlist) | 获取驱动列表             |
| [GetUIoTCoreDriverUpdateURL](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoredriverupdateurl) | 获取驱动更新上传的URL    |
| [GetUIoTCoreDriverUploadURL](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoredriveruploadurl) | 获取驱动上传URL          |
| [CreateUIoTCoreEdgeDriverBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=createuiotcoreedgedriverbind) | 网关绑定驱动             |
| [CreateUIoTCoreMultiDriverBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=createuiotcoremultidriverbind) | 批量绑定网关和驱动       |
| [GetUIoTCoreEdgeDriverList](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoreedgedriverlist) | 获取网关绑定驱动列表     |
| [DeleteUIoTCoreEdgeDriverBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=deleteuiotcoreedgedriverbind) | 删除网关和驱动绑定       |
| [GetUIoTCoreBindableProductList](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcorebindableproductlist) | 获取可绑定产品列表       |
| [GetUIoTCoreBindableDeviceList](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcorebindabledevicelist) | 获取可绑定设备列表       |
| [CreateUIoTCoreDriverSubDevBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=createuiotcoredriversubdevbind) | 驱动绑定子设备           |
| [CreateUIoTCoreMultiSubDevBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=createuiotcoremultisubdevbind) | 批量创建驱动和子设备绑定 |
| [GetUIoTCoreDriverSubDevList](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=getuiotcoredriversubdevlist) | 获取驱动子设备列表       |
| [DeleteUIoTCoreDriverSubBind](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=deleteuiotcoredriversubbind) | 删除驱动和子设备绑定     |
| [ModifyUIoTCoreEdgeDriverConf](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=modifyuiotcoreedgedriverconf) | 修改网关驱动配置         |
| [ModifyUIoTCoreDriverSubDevConf](https://docs.ucloud.cn/uiot-edge/api_list/subdev_driver_access?id=modifyuiotcoredriversubdevconf) | 修改驱动的子设备配置     |



## 添加函数计算

| API名称                                                      | 描述                 |
| ------------------------------------------------------------ | -------------------- |
| [CreateUIoTCoreFunction](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=createuiotcorefunction) | 创建函数             |
| [DeleteUIoTCoreFunction](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=deleteuiotcorefunction) | 删除函数             |
| [UpdateUIoTCoreFunction](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=updateuiotcorefunction) | 更新函数             |
| [GetUIoTCoreFunctionList](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=getuiotcorefunctionlist) | 获取函数列表         |
| [BindUIoTCoreFunctionToEdge](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=binduiotcorefunctiontoedge) | 绑定函数至边缘网关   |
| [GetUIoTCoreEdgeFunctionList](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=getuiotcoreedgefunctionlist) | 获取边缘网关函数列表 |
| [UnBindUIoTCoreFunctionFromEdge](https://docs.ucloud.cn/uiot-edge/api_list/edge_computing?id=unbinduiotcorefunctionfromedge) | 将函数从边缘网关解绑 |



## 添加本地应用





## 设置消息路由

| API名称                                                      | 描述             |
| ------------------------------------------------------------ | ---------------- |
| [CreateUIoTCoreMessageRouter](https://docs.ucloud.cn/uiot-edge/api_list/message_route?id=createuiotcoremessagerouter) | 增加消息路由     |
| [DeleteUIoTCoreMessageRouter](https://docs.ucloud.cn/uiot-edge/api_list/message_route?id=deleteuiotcoremessagerouter) | 删除消息路由     |
| [GetUIoTCoreMessageRouterList](https://docs.ucloud.cn/uiot-edge/api_list/message_route?id=getuiotcoremessagerouterlist) | 获取消息路由列表 |
| [ModifyUIoTCoreMessageRouter](https://docs.ucloud.cn/uiot-edge/api_list/message_route?id=modifyuiotcoremessagerouter) | 修改消息路由     |



## 远程运维管理

| API名称                                                      | 描述                     |
| ------------------------------------------------------------ | ------------------------ |
| [GetUIoTCoreDeviceResourceData](https://docs.ucloud.cn/uiot-edge/api_list/remote_maintaince?id=getuiotcoredeviceresourcedata) | 获取网关设备监控资源数据 |







