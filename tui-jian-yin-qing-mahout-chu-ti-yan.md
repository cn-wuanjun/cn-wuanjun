# 推荐引擎Mahout初体验

## Mahout是什么?

Mahout是一个算法库，集成了很多算法 Apache Mahout是Apache旗下的一个开源项目，提供一些可拓展的机器学习领域经典算法的实现， 旨在帮助开发人员更加方便快捷地创建智能应用程序 Mahout项目目前已经有了多个公共发行版本。Mahout包含许多实现，包括聚类、分类、推荐过滤、频繁子项目挖掘 通过使用Apache Hadoop库，Mahout可以有效地拓展到Hadoop集群。

> 算法介绍参考资料: [Mahout的taste推荐系统里的几种Recommender分析](https://blog.csdn.net/zhoubl668/article/details/13297583)

其中和我们业务贴近的是协同过滤算法(用户协同/物品协同),所以首先尝试了这两种算法.

## 如何使用

初始配置可以参考: [官方下载安装说明](https://mahout.apache.org/general/downloads)

我这里使用的是引入依赖的方式,需要注意的是引入依赖的方式是不需要下载安装mahout的,只需要当普通jar包使用即可

### 修改项目文件

`pom.xml`

```xml
<properties>
    <mahout.version>0.13.0</mahout.version>
</properties>


<dependency>
    <groupId>org.apache.mahout</groupId>
    <artifactId>mahout-math</artifactId>
    <version>${mahout.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.mahout</groupId>
    <artifactId>mahout-hdfs</artifactId>
    <version>${mahout.version}</version>
</dependency>
<dependency>
    <groupId>org.apache.mahout</groupId>
    <artifactId>mahout-examples</artifactId>
    <version>${mahout.version}</version>
</dependency>
```

### 准备模型数据

`case_collect.csv` 这里用的是用户收藏行为数据,从数据库中导出部分数据,数据格式需要符合`userID,itemID[,preference[,timestamp]]`.

> 其中分隔符不可省略
>
> 符合: 123,456,,129050099059
>
> 不符合: 123,456,129050099059

```sql
SELECT
    account_id,
    case_id,
    NULL,
    UNIX_TIMESTAMP(update_time) * 1000
FROM
    `case_collect`
WHERE
    account_id NOT LIKE '%@%'
```

### Java代码部分

```java
package com.znzmo.manager.controller;

import com.google.common.collect.Lists;
import io.swagger.annotations.Api;
import lombok.extern.slf4j.Slf4j;
import org.apache.mahout.cf.taste.common.TasteException;
import org.apache.mahout.cf.taste.impl.model.GenericBooleanPrefDataModel;
import org.apache.mahout.cf.taste.impl.model.file.FileDataModel;
import org.apache.mahout.cf.taste.impl.neighborhood.NearestNUserNeighborhood;
import org.apache.mahout.cf.taste.impl.recommender.GenericItemBasedRecommender;
import org.apache.mahout.cf.taste.impl.recommender.GenericUserBasedRecommender;
import org.apache.mahout.cf.taste.impl.similarity.LogLikelihoodSimilarity;
import org.apache.mahout.cf.taste.model.DataModel;
import org.apache.mahout.cf.taste.neighborhood.UserNeighborhood;
import org.apache.mahout.cf.taste.recommender.RecommendedItem;
import org.apache.mahout.cf.taste.recommender.Recommender;
import org.apache.mahout.cf.taste.similarity.ItemSimilarity;
import org.apache.mahout.cf.taste.similarity.UserSimilarity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.io.File;
import java.io.IOException;
import java.util.List;

@RestController
@Slf4j
@RequestMapping("/mahout")
@Api(value = "Mahout推荐")
public class MahoutDevController {
    @GetMapping("/item")
    public List<RecommendedItem> mahout(@RequestParam String userId) throws IOException {
        //数据模型
        String path = "/Users/qianbingxu/Downloads/case_collect.csv";
//        File file = new File(path);
        DataModel model;
        List<RecommendedItem> list = Lists.newArrayList();
        File cvsFile;
        try {
            long startTime = System.currentTimeMillis();
//            File bookCsvFile = ResourceUtils.getFile("classpath:csv/book_cvs_file.csv");
            cvsFile = new File(path);
            model = new GenericBooleanPrefDataModel(
                    GenericBooleanPrefDataModel
                            .toDataMap(new FileDataModel(cvsFile)));
            ItemSimilarity item = new LogLikelihoodSimilarity(model);
            //物品推荐算法
            Recommender r = new GenericItemBasedRecommender(model, item);
            list = r.recommend(Long.parseLong(userId), 10);
            list.forEach(System.out::println);
            long endTime = System.currentTimeMillis();
            log.info((endTime - startTime) + "时间");
        } catch (TasteException e) {
            e.printStackTrace();
        }

        return list;
    }

    @GetMapping("/user")
    public List<RecommendedItem> mahoutUser(@RequestParam String userId) {
        //数据模型
        DataModel model;
        List<RecommendedItem> list = Lists.newArrayList();
        File cvsFile;
        String path = "/Users/qianbingxu/Downloads/case_collect.csv";
        try {
            long startTime = System.currentTimeMillis();
            cvsFile = new File(path);
            model = new FileDataModel(cvsFile);
            //用户相似度算法
            UserSimilarity userSimilarity = new LogLikelihoodSimilarity(model);
            UserNeighborhood neighborhood = new NearestNUserNeighborhood(20, userSimilarity, model);
            Recommender r = new GenericUserBasedRecommender(model, neighborhood, userSimilarity);

            list = r.recommend(Long.parseLong(userId), 10);
            list.forEach(System.out::println);
            long endTime = System.currentTimeMillis();
            log.info((endTime - startTime) + "");
        } catch (IOException | TasteException e) {
            e.printStackTrace();
        }
        return list;
    }
}
```

### 获取推荐数据

最后请求对应的两个接口即可获取到推荐数据

```
RecommendedItem[item:15141545, value:1.0]
RecommendedItem[item:15376407, value:1.0]
RecommendedItem[item:13017549, value:1.0]
RecommendedItem[item:15286618, value:1.0]
RecommendedItem[item:15368495, value:1.0]
```