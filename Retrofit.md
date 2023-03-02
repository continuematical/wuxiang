# 简介

支持RESTful API设计风格，网络请求实际工作由OkHttp完成，Retrofit主要负责请求接口的包装；

## RESTful API

即表述性状态转移，只是风格没有标准，其核心是一切资源（数据加表现形式的集合），任何可命名的抽象概念都可以定义为一个资源，符合REST风格的API即为RESTful API；

## RESETful 对资源的操作

***POST***

在服务器新建一个资源，对应资源操作是INSERT，非幂等且不安全；

***GET***

从服务器中取出资源，对应资源操作是SELECT，幂等且安全；

***DELETE***

从服务器删除资源，对应资源操作是DELETE，幂等且不安全；

***PUT***

在服务器更新资源，客户端提供改变后的完整资源，对应资源操作是UPDATE，幂等且不安全；

*安全*是否会修改服务器的状态；

*幂等*返回数据是否相等；

# Retrofit的注解

### HTTP请求方法注解

### 标记类注解

#### FormUrlEncoded

#### Multipart

#### Streaming

代表响应的数据以流的形式返回，否则会存储到内存中；

### 参数类注解

@Path

动态配置Uri地址

@Query

动态指定查询条件

@QueryMap

动态指定查询条件组