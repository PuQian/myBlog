---
title: 【 数据库 】mybatis
date: 2016-03-13 16:17:04
tags: 数据库
---

## 模糊查询 like 通配符
`-`：表示任意单个字符
`%`：表示任意多个字符

## limit 用法
select * from table limit m, n // 取出从第 m 条开始的 n 条记录


```
<select id="totalRec" resultType="java.lang.Integer" parameterType="java.lang.String">
    select count(*) from grocery_item 
    <if test="_parameter != null" >
        where NAME like CONCAT('%',#{NAME,jdbcType=VARCHAR},'%')
    </if>
</select>

<select id="getRec" resultMap="BaseResultMap" parameterType="map">
    select * from grocery_item  
    <if test="NAME != null" >
        where NAME like CONCAT('%',#{NAME,jdbcType=VARCHAR},'%')
    </if>
        limit #{ST}, #{EACH}
</select>
```

<br/>

