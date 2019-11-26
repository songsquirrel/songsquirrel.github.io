---
title: MyBatis二级缓存应用-Redis
tags: MyBatis
---

&nbsp;　　前两天看的mybatis缓存机制，今天要将项目中的一些读取慢悠悠的数据放到缓存中，起初是想直接放到Redis里面，但是考虑到每个更新删除添加的接口都需要调用一下（虽然也可以， 但是就是发自内心的抵触~），头发掉了几根， 然后就有了这篇文章。  

### 一、概述 
　　由于项目中采用的为SpringCloud分布式服务，而Mybatis默认的二级缓存在分布式服务中会导致读取脏数据，所以考虑将Mybatis的二级缓存保存在Redis上。  
> 1. jar包下载： mybatis-redis-beta2.jar
> 2. 带beta的jar包可能会让人不那么安心，可以考虑copy代码，然后自己修改。

### 二、实现  

1. mybatis配置文件中开启缓存  
2. mapper.xml文件中使用 \<cache> 开启mybatis二级缓存  
3. 如果使用jar包的方式， 这个jar包会默认获取redis.properties配置文件的内容，如想自定义修改内容，可以复制代码，修改。

```xml
<mapper namespace="">
  <!-- type: 自定义的缓存bean（可选） -->
  <cache type=""/>
</mapper>
```
3. 涉及到多表联合查询的数据，可以使用\<cache-ref>来让多个mapper文件共用一个缓存  

```xml
<mapper namespace="">
  <cache-ref namespace="要去共用的那个缓存的namespace">
<mapper>
```

> ps: \<cache/>和\<cache-ref>标签一个xml文件中只需要两个中的一个。  
\<cache/>代表这个mappper的所有操作共用一个缓存；  
\<cache-ref>代表这个mapper的所有操作使用namespace指向的那个缓存。

### 三、总结  
> 没想到前两天刚学的知识，今天就用到了，还是多学点东西好啊。