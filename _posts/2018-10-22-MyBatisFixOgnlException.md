---
layout: post
title:  "MyBatis异常解决 '...OgnlException: source is null for getProperty...'"
categories: MyBatis
tags: MyBatis
author: MayerFang
---

* content
{:toc}

>MyBatis异常解决 '...OgnlException: source is null for getProperty...'





**Outline**
- [MyBatis异常解决 '...OgnlException: source is null for getProperty...'](#MyBatis异常解决 '...OgnlException: source is null for getProperty...')



---

# MyBatis异常解决 MyBatis异常解决 '...OgnlException: source is null for getProperty...'

标签：MyBatis

- 异常信息：
```
Cause: org.apache.ibatis.ognl.OgnlException: source is null for getProperty(null, "agent.id")
```
- 原因：在AgencyMapper.java中使用@Param注解指定命名,当传入为空时未做null判断，则抛出此异常
```
@Mapper
public interface AgencyMapper{
	List<User> queryAgents(@Param("pageParam")PageParam pageParam,@Param("agent")User agent);
}
```
- 解决方法：在外加一层<if test="agent!=null">做判断
```
AgencyMapper可以继续使用@Param注解指定命名
agency.xml中修改后
<select id="queryAgents" resultType="user">
        select <include refid="userField"/>
        from user a where type = 2
        <if test="agent!=null">
            <if test="agent.id!=null">
                and a.id = #{agent.id}
            </if>
        </if>
		...
```
- 其他解决方法：不使用@Param注解，当传入单个对象为null时不会抛出此异常，xml文件中也不用使用名称前缀