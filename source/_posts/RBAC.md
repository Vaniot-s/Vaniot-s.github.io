---
title: RBAC权限管理
mathjax: true
date: 2018-03-16 20:58:28
tags: [访问控制模型,数据库授权设计]
categories: web
---
### 概念
<b>RBAC</b>(Role-Based Access Control):基于角色的权限访问控制，在用户与权限之间添加角色层，权限与角色相关联，用户通过角色的成员而得到该角色的权限，角色依据完成各种工作而创作，用户依据其责任和资格被指派相应的角色，用户的角色可很容易的更改，从而化简权限管理。
<b>RBAC三原则:</b>
     1.最小权限:将角色配置成其完成任务所需要的最小权限集
     2.责任分离：通过相互独立互斥的角色来共同完成任务。
     3.数据抽象:通过权限的抽象来体现，RBAC支持的数据抽象程度与RBAC的实现细节有关。
  
  > 总之:用户实质并无权限，在成为某一角色后才拥有了特定权限,一个用户可有多个角色，一个角色可有多个用户，一个权限可有多个角色，一个角色可有多个权限。
  
  <!--more-->
### RBAC96模型族
 RBAC96 包括RBAC0~RBAC3四个概念性模型。
 
 ![RBAC](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC.png?raw=true)
#### RBAC0符合RBAC概念系统的最低要求
   RBAC中包含用户(USERS),角色(ROLES)，目标(OBS)，操作(OPS
),许可(PRMS)五个基本元素，RBAC0指出了角色，用户，访问权限和会话的关系。
![rbac0-1](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC0-1.png?raw=true)
![RBAC0](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC0.png?raw=true)
每个用户至少扮演一个角色，每个角色至少有一个权限，对于完全不同的角色可以分配相同的权限。一个用户可以创建并激活多个角色，从而获得相应的访问权限，用户可以在会话中更改激活角色，用户也能够主动结束一个会话。用户与角色、角色与权限均是多对多的关系。
#### RBAC1 模型是在基础的模型上加上了角色继承的关系
RBAC1基于RBAC0,引入了角色间的继承关系(角色的上下级关系),角色的继承可分为一般继承(绝对偏序关系,允许角色的多继承)和受限继承(树结构，实现角色的单继承)
![RBAC1](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC1.png?raw=true)
![RBAC1-1](https://github.com/vaniot-s/picture/blob/master/rbac/rBAC1.png?raw=true)
####  RBAC2 模型是在基础模型的基础上加上了责任分离的关系
RBAC2 在RBAC0的基础上引入了SSD(静态职责分离)和DSD(动态职责分离),用于限制角色被赋予权限、用户被赋予角色、用户激活角色应遵循的强制性规则。

- SSD
  - 互斥角色规则:同一个用户在两个互斥的角色中只能选择一个
  - 基数规则: 一个用户拥有的角色是有限的，一个角色拥有的权限也是有限的
  - 先决规则: 用户想获得高级的角色，首先必须拥有低级的角色
- DSD
  - 运行时互斥规则:一个用户可以拥有两个角色，但运行时只能激活一个角色

![RBAC2](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC2.png?raw=true)

#### RBAC3 模型是包含了 RBAC1 和 RBAC2 模型
RBAC3是最全面，最复杂的权限管理。
![RBAC3](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC3.png?raw=true)
![RBAC3](https://github.com/vaniot-s/picture/blob/master/rbac/RBAC3-1.png?raw=true)
#### 扩展:用户组
在RBAC模型的基础之上，增加用户组的概念 ，将用户加入用户组，用户拥有用户组的权限及本身的权限
![RBAC3](https://github.com/vaniot-s/picture/blob/master/rbac/RBACUGROUP.png?raw=true)
### RBAC的优缺点
RBAC模型没有提供操作顺序控制机制。使得RBAC模型很难应用关于那些要求有严格操作次序的实体系统。
### 基于RBAC模型的权限验证框架与应用
- Apache Shiro
- spring Security
- SELinux