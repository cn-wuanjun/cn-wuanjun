---
layout: post
title:  "Springboot集成IKAnalyer分词器"
categories: jekyll update
---
## 在pom.xml中添加依赖
```xml
<!-- ikanalyzer 中文分词器  -->
<dependency>
    <groupId>com.janeluo</groupId>
    <artifactId>ikanalyzer</artifactId>
    <version>2012_u6</version>
    <exclusions>
        <exclusion>
            <groupId>org.apache.lucene</groupId>
            <artifactId>lucene-core</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.lucene</groupId>
            <artifactId>lucene-queryparser</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.lucene</groupId>
            <artifactId>lucene-analyzers-common</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!--  lucene-queryparser 查询分析器模块 -->
<dependency>
    <groupId>org.apache.lucene</groupId>
    <artifactId>lucene-queryparser</artifactId>
    <version>9.0.0</version>
</dependency>
```
## 新建工具类
```java
package com.znzmo.component;

import com.github.pagehelper.util.StringUtil;
import com.google.common.collect.Lists;
import org.springframework.stereotype.Component;
import org.wltea.analyzer.core.IKSegmenter;
import org.wltea.analyzer.core.Lexeme;

import java.io.StringReader;
import java.util.ArrayList;
import java.util.List;

@Component
public class IKAnalyzerSupport {
    public List<String> iKSegmenterToList(String string){
        List<String> list=new ArrayList<>();
        try{
            if(StringUtil.isEmpty(string)){
                return Lists.newArrayList();
            }
            StringReader sr=new StringReader(string);
            IKSegmenter ik=new IKSegmenter(sr, true);
            Lexeme lex;
            while((lex=ik.next())!=null){
                String lexemeText=lex.getLexemeText();
                if(lexemeText.length()>=2){
                    list.add(lexemeText);
                }
            }
        }catch (Exception e){
            e.printStackTrace();
        }
        return list;
    }
}
```
## 添加自定义配置
在resource目录下新建文件
- IKAnalyzer.cfg.xml
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
    <properties>
        <comment>IKAnalyzer扩展配置</comment>
        <!--用户的扩展字典 -->
        <entry key="ext_dict">extend.dic</entry>
        <!--用户扩展停止词字典 -->
        <entry key="ext_stopwords">stopword.dic</entry>
    </properties>
    ```
- extend.dic 扩展词库
    ```
    侘寂
    ```
- stopword.dic 停用词库
  ```
  的
  ```
