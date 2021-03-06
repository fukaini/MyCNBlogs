<h2>《大型网站技术架构-核心原理与案例分析》 :books: </h2> 

> 李智慧 著       电子工业出版社

```txt
 1.大型网站架构演化发展历程：
   (1) 初始阶段的网站架构;
   (2) 应用服务和数据服务分离;
   (3) 使用缓存改善网站性能;
   (4) 使用应用服务器集群改善网站的并发处理能力;
   (5) 数据库读写分离;
   (6) 使用反向代理和 CDN 加速网站响应;
   (7) 使用分布式文件系统和分布式数据库系统;
   (8) 使用 NoSQL 和搜索引擎;
   (9) 业务拆分;
   (10) 分布式服务。
   
 2.网站架构模式：分层、分割、分布式、集群、缓存、异步、冗余、自动化、安全。
 
 3.大型网站核心架构要素：性能、可用性、伸缩性、扩展性、安全性。
 
 4.高可用网站的软件质量保证：网站发布、自动化测试、预发布验证、代码控制、自动化发布、灰度发布。
 
 5.应用服务器集群的伸缩性设计：
   (1) HTTP 重定向负载均衡;
   (2) DNS 域名解析负载均衡;
   (3) 反向代理负载均衡;
   (4) IP 负载均衡;
   (5) 数据链路层负载均衡;
   (6) 负载均衡算法。
 
 6.网站性能优化：         
  1)Web前端性能优化        
    (1) 浏览器访问优化：
     -减少 http 请求.
     -使用浏览器缓存.   
     -启用压缩. 
     -CSS放在页面最上面, JavaScript放在页面最下面.
     -减少Cookie传输.
    (2) CDN(内容分发网络)加速：缓存静态资源.
    (3) 反向代理：安全功能，配置缓存加速Web请求，负载均衡.
  2)应用服务器性能优化：
    (1) 分布式缓存.
      -网站性能优化第一定律：优先考虑使用缓存优化性能.
      -合理使用缓存：缓存预热，缓存穿透.
      -分布式缓存架构：
        a.以 JBoss Cache 为代表的需要更新同步的分布式缓存；
        b.以 Memcached 为代表的不互相通信的分布式缓存.
    (2) 异步操作：使用消息队列将调用异步化，削峰作用.
    (3) 使用集群.
    (4) 代码优化.
      -多线程：启动线程数＝[任务执行时间 / (任务执行时间 - IO 等待时间)] × CPU 内核数.
        a.将对象设计成无状态对象；
        b.使用局部对象；
        c.并发访问资源时使用锁.
      -资源复用：单例( Singleton ) 和对象池(Object Pool).
      -数据结构：字符串 Hash 散列算法 Time33 算法，即对字符串逐字符迭代乘以33，求得 Hash 值：
        hash(i) = hash(i-1) * 33 + str[i]
       原始字符串 (MD5) → 信息指纹 (hash) → HashCode.
      -垃圾回收.
  3)存储性能优化：
    (1) 机械硬盘 vs 固态硬盘(SSD).
    (2) B+ 树 vs LSM 树.
       关系型数据库多采用两级索引的B+树.
       B+ 树是一种专门针对磁盘存储而优化的N叉排序树，以树节点为单位存储在磁盘中，
     从根开始查找所需数据所在的节点编号和磁盘位置，将其加载到内存中然后继续查找，直到找到所需的数据.
       NoSQL产品多采用 LSM 树作为主要数据结构.
       LSM 树可以看作是一个N阶合并树. 数据写操作(包括插入、修改、删除)都在内存中进行，并且会创建一个新纪录
     (修改会记录新的数据值，而删除会记录一个删除标志)，这些数据在内存中仍然还是一棵排序树，当数据量超过设定的
     内存阈值后，会将这棵排序树和磁盘上最新的排序树合并。
    (3) RAID(廉价磁盘冗余阵列) vs HDFS(Hadoop 分布式文件系统)
      HDFS 两类重要的服务器角色：NameNode (名字服务节点)和 DataNode (数据存储节点).
      HDFS 配合 MapReduce 等并行计算框架进行大数据处理时，可以在整个集群上并发读写访问所有的磁盘，无需 RAID 支持.
 
 7.网站应用攻击与防御：
  1) XSS 攻击：即跨站点脚本攻击(Cross Site Script)，指黑客通过篡改网页，注入恶意 HTML 脚本，在用户浏览网页时，
  控制用户浏览器进行恶意操作的一种攻击方式.
   常见的 XSS 攻击类型有两种：一种是反射型，另一种是持久型.
   防范 XSS 攻击的手段：过滤和消毒(如">"转义为"＆gt")、HttpOnly(Cookie).
  2) 注入攻击：主要有两种形式，SQL 注入攻击和 OS 注入攻击.
   防范注入攻击的手段：请求参数消毒(过滤请求数据中的SQL，如"drop table"，
   "\b(?:update\b.*?\bset|delete\b\W*?\bfrom)\b"等)、参数绑定.
  3) CSRF 攻击：(Cross Site Request Forgery)跨站点请求伪造，攻击者通过跨站请求，以合法用户的身份进行非法操作，
  如转账交易、发表评论等.
    防范 CSRF 攻击的手段主要是识别请求者身份：表单Token、验证码、Referer check.
  4) 其他攻击和漏洞：Error Code、HTML注释、文件上传、路径遍历.
  5) Web 应用防火墙：ModSecurity是一个开源的 Web 应用防火墙，探测攻击并保护 Web 应用程序，它采用处理逻辑与攻击规则
  集合分离的架构模式.
  6) 网站安全漏洞扫描.
```
