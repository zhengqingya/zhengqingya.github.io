---
layout: post
title: 'ElasticSearch入门 - Java客户端实现增删改查'
date: 2019-01-06
categories: 技术
tags: ElasticSearch
---

<strong><font color=SlateGray>前言：</font> </strong> es java客户端有 原生ESTransport Client方式（相当于写jdbc代码，麻烦，不采用）、spring data es（spring对Es的数据访问，类似于spring data jpa对db访问，采用！）两种方式。

**<font color=red>springboot项目整合es：</font>**

**1、创建springboot项目：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191419303.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
**2、导入spring data es依赖：**

```java
<!--springboot 对 spring data es 的支持-->
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
</dependency>
```

**3、配置** 
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191446426.png)
```yaml
spring:
  data:
    elasticsearch:
      cluster-name: elasticsearch
      cluster-nodes: 127.0.0.1:9300  # 注：9200->图形界面端、9300->代码端
```

**4、启动类：**

```java
@SpringBootApplication
public class ElasticsearchApplication {
    public static void main(String[] args) {
        SpringApplication.run(ElasticsearchApplication.class, args);
    }
}
```

**5、测试**

> <strong>注：</strong>测试的时候不要忘记启动elasticsearch服务端和kibana哦！！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191538375.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
> 测试一下： http://127.0.0.1:5601
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191714410.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

① 创建索引 并 建立类型映射：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191834655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)
```java
//indexName:索引库     type:类型
@Document(indexName = "zq_test",type = "user")
public class User {
    @Id
    private Long id;
    @Field(type = FieldType.Keyword)
    private String name;
    @Field(type = FieldType.Text,analyzer = "ik_max_word",searchAnalyzer = "ik_max_word")
    private String intro;
    @Field(type = FieldType.Integer)
    private Integer age;
    //getter/setter方法、toString方法...
    public User() { }
    public User(Long id, String name, String intro, Integer age) {
        this.id = id;
        this.name = name;
        this.intro = intro;
        this.age = age;
    }
}
```

```java
@Repository
public interface UserRepository extends ElasticsearchRepository<User,Long> { }
```

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ElasticsearchApplication.class)
public class ESTest {
    @Autowired
    private ElasticsearchTemplate template;

    @Test  //创建索引,并且建立类型映射
    public void testPre() throws Exception {
        template.createIndex(User.class); //创建索引
        template.putMapping(User.class);   //类型映射-自定义映射
    }
}
```

运行测试
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121191857892.png)
查看类型映射： <font color=SkyBlue>GET zq_test/_mapping/user</font>
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121192019132.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

> <strong>注：</strong>如果报以下错误：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121192131546.png)
>
> <strong>解决方法：</strong>修改elasticsearch配置文件：   -  如果这样修改后还是不行的话可能就要排除是不是版本问题了哦！
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121192150667.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)

②  CRUD：

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = ElasticsearchApplication.class)
public class ESTest {

    @Autowired
    private UserRepository repository;

    @Test   //添加1个
    public void testAdd() throws Exception {
        User user = new User(1L, "zheng qing", "zheng qing is a programmer!", 18);
        repository.save(user);
    }

    @Test   //批量添加
    public void testBatchAdd() throws Exception {
        List<User> list = new ArrayList<>();
        for (int i = 0; i < 20; i++) {
            User user = new User(i + 2L, "zheng qing" + i + 1, "zheng qing is a programmer!", 18 + i );
            list.add(user);
        }
        repository.saveAll(list);
    }

    @Test //获取1个
    public void testGetOne() throws Exception {
        System.out.println(repository.findById(1L));
    }

    @Test   //获取全部
    public void testGetAll() throws Exception {
        Iterable<User> all = repository.findAll();
        for (User user : all) {
            System.out.println(user);
        }
    }

    @Test //新增、修改 （判断是否有id即可~）
    public void testUpdate() throws Exception {
        Optional<User> byId = repository.findById(1L);
        User user = byId.get();
        System.out.println(user);
        user.setName("zq");
        repository.save(user);
        System.out.println(repository.findById(1L));
    }

    @Test  //删除
    public void testDel() throws Exception {
        repository.deleteById(1L);
        //repository.deleteAll();;
    }
}
```

③ 高级查询(过滤)+分页+排序

```java
@Test //DSL查询与过滤
public void testNativeSearchQueryBuilder() throws Exception {
    NativeSearchQueryBuilder builder = new NativeSearchQueryBuilder();
    BoolQueryBuilder bool = QueryBuilders.boolQuery();
    //模糊查询
    bool.must(QueryBuilders.matchQuery("intro", "zheng"));
    //精确过滤
    List<QueryBuilder> filters = bool.filter();
    filters.add(QueryBuilders.rangeQuery("age").gte(0).lte(200));
    builder.withQuery(bool); //query bool must(filter)
    //排序
    builder.withSort(SortBuilders.fieldSort("age").order(SortOrder.ASC));
    //分页   注：当前页从0开始
    builder.withPageable(PageRequest.of(0, 10));
    //截取字段
    builder.withSourceFilter(new FetchSourceFilter(new String[]{"name", "age"}, null));
    //构造查询条件
    NativeSearchQuery query = builder.build();
    //查询
    Page<User> page = repository.search(query);
    System.out.println("总数:" + page.getTotalElements());
    System.out.println("总页数:" + page.getTotalPages());
    for (User user : page.getContent()) {
        System.out.println(user);
    }
}
```

------

<strong>最后附上源码：</strong> https://pan.baidu.com/s/1Mi64ludl3MOhg3RrXUEQkg
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190121192456218.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MjI1NTU4,size_16,color_FFFFFF,t_70)



