![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221121.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=Z5ojM2ZbTY032ssn9Ov/KSbBnUQ=)


![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221156.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=0pqZnTZRz0eP7IGSUxaMYj/oWnc=)

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221218.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000001&Signature=ugtmmu6dFPJ6GD1EFmZyWv4CRVY=)

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221415.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=UVVZHLOx3Mkm7/e2ijZSFe//0GE=)


![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221439.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=ya5nGMzXF4c0ibHW9uEhaoDVccI=)


![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902221601.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=0zVi7AUJ2jOb69q3ZduVls7DlRQ=)



# sonic编译


## 1.整体架构
### 1.1编译的目的
SONiC系统的架构由各种模块组成,这些模块通过集中式和可展的基础设施相互交互。
SONICbuildimage是一个基于GNUmake的自动化构建环境,我们编译的目的就是为了生成一个完整的、可定制的网络操作系统镜像(sonic-broadcom.bin),然后这个镜像可以直接安装到交换机上运行。一个完整的镜像要集成各种必要的软件包、容器、工具和配置。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902222905.png" alt="image.png" style="zoom:80%;" />
编译还要将必要的软件包、容器镜像和配置文件生成然后整合安装到最终的镜像文件中,这样才能提供开放、灵活的网络操作系统功能。

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223136.png" alt="image.png" style="zoom:80%;" />

从这个文件的结构我们就可以看出sonic是按照功能和构建过程的不同阶段来划分这个文件结构的。 
- Makefile:是一个对sonic-slave docker image的包装器 
- slave.mk:是实际的makefile。它为目标群体定义了一组规则(稍后将详细介绍)。你可以在这 里找到一个make规则,用于recipe中定义的每个目标。 
- sonic-slave-bullseye:这个文件夹定义SONiC构建环境的Docker容器,这个文件夹包含了创 建容器所需要的文件,系统会用这个文件夹下Dockerfile.j2模板创建sonic-slave镜像 
- rules文件夹:包含了构建各种组件所需的规则和配置。它定义了"what to build"(要构建什么) 
- dockers文件夹:包含了SONiC中各个服务和组件的Docker容器定义。SONiC采用了微服务 架构,每个主要功能都被封装在独立的Docker容器中。 
- src文件夹:就是源代码文件的目录。 
- platform文件夹:提供不同交换机硬件平台所需的特定规则和目标,使SONiC能够在各种不同 的硬件上运行。 
- scripts文件夹:包含了一系列用于支持SONiC构建、配置和管理过程的辅助脚本,这些脚本 自动化了许多复杂的任务,确保构建过程的一致性和效率。 
- target文件夹:是SONiC构建过程的输出目录。它存储了构建过程中生成的各种文件和最终的 SONiC镜像。


![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223254.png)


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223320.png" alt="image.png" style="zoom:60%;" />

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223404.png)


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223427.png?" alt="image.png" style="zoom:60%;" />

<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223530.png" alt="image.png" style="zoom:60%;" />



# 动态端口分流


![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223703.png)


<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223732.png?" alt="image.png" style="zoom:60%;" />



![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223759.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=OwtXqc8webICF7zrcVybA9neV44=)



<img src="http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223851.png" alt="image.png" style="zoom:60%;" />

![image.png](http://yesho-web.oss-cn-hangzhou.aliyuncs.com/img/20240902223959.png?OSSAccessKeyId=LTAI5tBK1gnqyQLHK2d7sx6F&Expires=9000000000&Signature=WHLHW2mqYujUUgtwSUeIdyjNAbw=)




PBR(Policy-Based Routing)策略路由的应用场景如下：

1. **本地PBR应用场景**

本地PBR主要处理设备产生的流量路由选择：

```plaintext
常见场景：
1. 管理流量分离
   - SNMP流量走专用链路
   - Telnet/SSH管理流量走管理网络

2. 特定服务分流
   - NTP同步走专用时钟源链路
   - Syslog日志采集走专用通道

3. 多ISP接入
   - 内部业务流量走专线
   - Internet访问走公网链路
```

2. **接口PBR应用场景**

接口PBR主要处理流经设备的转发流量：

```plaintext
常见场景：
1. 差分服务
   - VIP用户走质量好的链路
   - 普通用户走普通链路

2. 链路负载分担
   - TCP流量走链路1
   - UDP流量走链路2

3. 多出口负载均衡
   - 访问国内网站走电信
   - 访问国外网站走联通
```

3. **配置示例**

```bash
# 本地PBR配置示例
ip local policy route-map LOCAL_PBR

route-map LOCAL_PBR permit 10
match ip address 101
set ip next-hop 192.168.1.1

# 接口PBR配置示例
interface GigabitEthernet0/1
 ip policy route-map INTERFACE_PBR

route-map INTERFACE_PBR permit 10
match ip address 102
set ip next-hop 192.168.2.1
```

4. **实现机制**

```plaintext
路由决策过程：
1. 匹配条件(Match Criteria)
   - 源地址/目的地址
   - 协议类型
   - 端口号
   - 报文长度
   
2. 设置动作(Set Action)
   - 下一跳
   - 出接口
   - IP优先级
   - 服务质量
```

5. **使用建议**

- 本地PBR：
  - 用于管理流量控制
  - 服务器双链路接入
  - 运维管理分离

- 接口PBR：
  - 用于用户流量控制
  - 多出口选路
  - QoS策略实施