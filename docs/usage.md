# kfp 使用指南

# 一、商品链接

[ArgoWorkflows](https://marketplace.huaweicloud.com/contents/992480da-64a3-4ba8-90cb-686d1832e96a#productid=OFFI1111485128289529856)

# 二、商品说明

kfp(Kubeflow pipelines)是使用 Kubeflow Pipelines SDK 构建的可复用的端到端 ML 工作流。kubeflow是一款机器学习 (ML) 工具包，致力于简化 Kubernetes 上 ML 工作流的部署，使其更加便捷、可移植且可扩展。

本商品通过 鲲鹏服务器 + Huawei Cloud EulerOS 2.0 64bit 进行安装部署。

# 三、商品购买

您可以在云商店搜索 **kfp**。

其中，地域、规格、推荐配置使用默认，购买方式根据您的需求选择按需/按月/按年，短期使用推荐按需，长期使用推荐按月/按年，确认配置后点击“立即购买”。

## 3.1、ECS控制台自定义购买示例

### 准备工作

在使用ECS控制台配置前，需要您提前配置好 **安全组规则**。

> **安全组规则的配置如下：**

* 入方向规则放通 CloudShell 连接实例使用的端口 `22`，以便在控制台登录调试
* 出方向规则一键放通

### 创建ECS

前提工作准备好后，选择 ECS 控制台配置跳转到[购买ECS](https://support.huaweicloud.com/qs-ecs/ecs_01_0103.html) 页面，ECS 资源的配置如下图所示：

#### 选择CPU架构和规格

1.根据镜像所适配的架构选择对应的架构

2.选择的规格要大于或等于说明文档中提供的最小规格

![image](images/img_10.png)

#### 选择镜像

1.点击市场镜像，进入市场进行列表

2.选择需要购买的商品的规格镜像

![img.png](images/img.png)

#### 其他参数配置

1.其他参数根据实际情况进行填写

2.安全组选择提前配置好的安全组

3.配置完成之后点击立即购买即可

![image](images/img_3.png)

**值得注意的是：**

* VPC 您可以自行创建
* 安全组选择 [**准备工作**](#准备工作)中配置的安全组；
* 弹性公网IP选择现在购买，推荐选择“按流量计费”，带宽大小可设置为5Mbit/s；
* 高级配置需要在高级选项支持注入自定义数据，所以登录凭证不能选择“密码”，选择创建后设置；
* 其余默认或按规则填写即可。


## 3.2、模板部署方式购买示例

### 购买商品

以购买“spark分布式计算引擎”做为示例，描述模板部署方式购买镜像商品全流程

#### 选择商品

1.在云商店通过商品名称搜索，“spark分布式计算引擎"，进入到商品页

2.选择好地域后，点击“立即购买”

![img_11.png](images/img_11.png)

#### 选择开通方式
1.勾选协议
2.模板配置开通

![image](images/img_4.png)

第一步不需要做额外修改，直接下一步
![image](images/img_8.png)

#### 参数配置

第二步进行参数配置，按需填写服务器规格参数后，点击下一步
![image](images/img_2.png)

第三步不需要做额外修改，直接下一步
![image](images/img_9.png)

第四步确认配置后，创建执行计划，点击 确定

![image](images/img_5.png)

![image](images/img_7.png)

创建完成之后点击部署，执行计划
![image](images/img_1.png)

如下图“Apply required resource success. ”即为资源创建完成
![image](images/img_6.png)

# 商品使用

## kfp 使用

### kfp使用示例

1.创建文件夹并切换到该目录
```shell
mkdir /kfp && cd /kfp
```
2.编辑python文件tesy.py，通过kfp sdk方式生成siample_test.yaml工作流测试
```shell
import kfp
from kfp import dsl
from kfp.dsl import component

# 使用@component 装饰器定义组件
@component
def generate_random() -> int:
	import random
	return random.randint(0,100)

@component
def print_message(number: int):
	print(f’Generated number: {number}’)

#定义管道
@dsl.pipeline(
	Name=’simple-pipeline’,
	Description=’A simple pipeline that generates a random number and prints it’
)
def simple_pipeline():
	random num = generate_random()
	print_message_op = print_message(number=random_num.output)

if __name__ == ‘__main__’:
	#编译管道
	kfp.compiler.Compiler().compile(
		pipeline_func=simple_pipeline,
		package_path=’simple-pipeline.yaml’
	)
	print(“Pipeline YAML generated successfully!”)


```
3.执行python test文件
```shell
python test.py 
```
4.上传simple-pipeline.yaml至kfp工作流

```shell
#api上传
curl -X POST -H “Content-Type: multipart/form-data” \
-F uploadfile=@simple-pipeline.yaml \
http://172.16.0.109:8888/apis/v1beta1/pipelines/upload
```
#页面web端上传
![image](images/img_15.png) 
5.在web端点击上传好的simple-pipeline.yaml，进入至详情页面后，点击“Create run”,创建成功后执行
![image](images/img_16.png)
6.执行成功结果如下图所示：
![image](images/img_17.png)

