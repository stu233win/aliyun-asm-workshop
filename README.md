## 2021年10月14日阿里云 云原生容器上海站


####  基于服务网格ASM的流量治理实践部分 [https://developer.aliyun.com/article/793680]

阿里云服务网格（Alibaba Cloud Service Mesh，简称ASM）提供了一个全托管式的服务网格平台，兼容于社区Istio开源服务网格，用于简化服务的治理，包括服务调用之间的流量路由与拆分管理、服务间通信的认证安全以及网格可观测性能力，从而极大地减轻开发与运维的工作负担。本文是关于服务网格ASM流量治理相关的workshop。


### 前提条件
#### 创建至少一个ASM实例，并添加至少一个ACK集群到该实例中。详情请参见创建ASM实例和添加集群到ASM实例。
#### 在ACK控制台获取ACK的kubeconfig 和ASM控制台获取ASM的kubeconfig

环境准备参考：
第一步：在ASM控制台，创建网格实例：
![image](https://user-images.githubusercontent.com/18081853/137580916-65ed3603-a00e-4cbc-8f4a-1991e52708b1.png)
第二步：添加kubernetes集群到服务网格ASM
![image](https://user-images.githubusercontent.com/18081853/137580945-0ce952ce-6621-45d5-93a9-95e1b200cbe0.png)
实践内容
本文用到的workshop用到的相关资源
1.部署应用并注入Sidecar代理
(1) 在ASM控制台开启Sidecar的自动注入或者
使用kubectl --kubeconfig asm_kubeconfig label default istio-injection = enabled
![image](https://user-images.githubusercontent.com/18081853/137580955-423f307e-3e1c-448e-b4e0-593b4fa317ec.png)
(2) 部署应用
 kubectl --kubeconfig ack_kubeconfig apply -f app.yaml
今天演示应用的部署架构，包括产品服务、增值服务（包括三个版本）、产品详情、风格转换服务四个多语言开发的应用，包括一个Ingress 入口网关和一个外部服务，并且通过服务网格ASM注入了Sidecar代理。
![image](https://user-images.githubusercontent.com/18081853/137580986-be4fc52c-6c5a-491b-92b4-bba8d1376cfa.png)

应用增值服务V1
![image](https://user-images.githubusercontent.com/18081853/137580995-3431418a-9461-4cf7-93c5-b83b5f92ea84.png)

关于增值服务V2
![image](https://user-images.githubusercontent.com/18081853/137581005-10ca55ce-1894-459f-91cc-acda044cee0e.png)

关于增值服务V3
![image](https://user-images.githubusercontent.com/18081853/137581015-f915a315-e106-4c72-ab63-061ec8ec85f8.png)

接下来的操作都可以使用ASM控制台或者Kubectl完成。
#### 2.部署入口网关访问应用
使用控制台一键部署网关访问我们的应用。
