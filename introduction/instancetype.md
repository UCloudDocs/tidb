# 实例类型

支持低延时的同可用区集群和机房级容灾的跨可用区集群。

## 跨可用区

> 此类型暂时不支持创建新实例

在TiDB原本三副本特性的高可用基础上，部署模式保证三副本同地域三可用区，进一步的提高受机房级别故障影响的可用性。

## 同可用区

为满足部分客户对于跨机房延时性能不能满足需求的场景，2020年4月推出了主打低延时特性同可用区部署的TiDB实例类型，三副本坐落在不同的宿主机用以保证高可用。

![](http://tidb-doc.cn-bj.ufileos.com/basic/create002.png)

为了满足小数据量用户控制内存使用上限的需求， 我们提供限制TiKV内存的选项。 默认不做限制， 按需使用。 当开启限制功能后，存储(TiKV)节点的总内存使用量会被限制在选定的范围内。

![](http://tidb-doc.cn-bj.ufileos.com/basic/create003.png)


### 限制TiKV内存 - 30G

将同时限制：

| 限制项     | 数量（个）    | 单节点内存（G）   | 单节点存储（G） |
| -------    | -------------| ----------- | --------- | 
| 计算节点   | 3            |  4          | NA        |
| 存储节点   | 3            |  10         | 200       |


### 限制TiKV内存 - 60G

将同时限制：

| 限制项     | 数量（个）    | 单节点内存（G）   | 单节点存储（G） |
| -------    | -------------| ----------- | --------- | 
| 计算节点   | 3            |     8       | NA        |
| 存储节点   | 3            |     20      | 400      |
