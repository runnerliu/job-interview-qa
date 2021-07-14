> 本文整理基础面试题目QA

### Python相关

1. 可迭代对象、迭代器、生成器
 - 可迭代对象：实现了 `__iter__()` 或 `__getitem__(index)` 方法，否则会抛出 `'X' object is not iterable`，如list、tuple、str、bytes、dict、set、collections.deque
 - 迭代器：实现了无参数的`__next__()`方法，返回序列中的下一个元素，如果没有元素了，就抛出`StopIteration`异常。`__iter__()` 返回迭代器本身。可以支持 for循环、逐行遍历文本文件、列表推导、字典推导、集合推导等。所有迭代器都是可迭代对象，反之不一定
 - 生成器：只要函数的定义体中有`yield`关键字，该函数就是生成器函数。调用生成器函数时，会返回一个生成器对象。也就是说，生成器函数是生成器工厂。迭代器和生成器都是为了惰性求值，避免浪费内存空间
2. python3中bytes和str的区别
 - Python2的字符串有两种：str 和 unicode，Python3的字符串也有两种：str 和 bytes。Python2 的 str 相当于 Python3 的bytes，而unicode相当于Python3的str。Python3里面的str是在内存中对文本数据进行使用的，bytes是对二进制数据使用的。str可以encode为bytes，但是bytes不一定可以decode为str。bytes一般来自网络读取的数据、从二进制文件（图片等）读取的数据、以二进制模式读取的文本文件(.txt, .html, .py, .cpp等)
3. 装饰器
```
import time

def timmer(flag):
    """
    :param flag: 接收装饰器的参数
    :return:
    """
    def outer_wrapper(func):
        """
        :param func: 接收被装饰的函数
        :return:
        """
        # 接收被装饰函数的参数
        def wrapper(*args, **kwargs):
            """
            :param args: 收集被装饰函数的参数
            :param kwargs: 收集被装饰函数的关键字参数
            :return:
            """
            if flag == "true":
                start_time = time.time()
                # 调用被装饰的函数
                result = func(*args, **kwargs)
                # 让进程睡一秒
                time.sleep(1)
                stop_time = time.time()
                print("{func} spend {time} ".format(func="add", time=stop_time - start_time))
                return result
            else:
                print("Unexpected ending")
        return wrapper
    return outer_wrapper
```
4. list的底层实现方式，存储方式（地址空间连续），插入复杂度O(n)
5. GIL -> [Python系列 - 计算密集型任务和I/O密集型任务](https://runnerliu.github.io/2017/07/15/jsmjiomj/)
6. 常用标准库
 - os sys traceback json time datetime math hashlib logging threading queue copy 等
7. 常用第三方库
 - redis sqlalchemy requests tornado apscheduler celery pymysql pyjwt pykafka 等
8. 生成器、迭代器的实现
9. django、tornado、flask实现上的区别，并发处理方式
10. tornado异步是如何实现的 -> [Tornado异步非阻塞详解](https://www.jianshu.com/p/22b92e92aca5)
11. python协程和go协程的实现及区别
12. 如何理解面向对象，对象的特性
 - 面向对象是将现实问题构建关系，然后抽象成类，给类定义属性和方法后，再将类实例化成实例，通过访问实例的属性和调用方法来进行使用
 - 封装、继承、多态
13. tornado的socket处理
14. tornado异步非阻塞的实现方式
15. mysql连接类代码
```
import traceback

from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker


class MysqlUtils(object):

    def __init__(self, db_config):
        self.db_config = db_config
        self.host = self.db_config['host']
        self.port = self.db_config['port']
        self.name = self.db_config['name']
        self.user = self.db_config['user']
        self.password = self.db_config['password']
        self.charset = self.db_config.get('charset', None)
        if self.charset:
            self.engine = create_engine(
                "mysql+pymysql://{}:{}@{}:{}/{}?charset={}".format(
                    self.user, self.password, self.host, self.port, self.name, self.charset),
                encoding="utf-8",
                echo=False,
                pool_recycle=3600,
                pool_size=30)
        else:
            self.engine = create_engine(
                "mysql+pymysql://{}:{}@{}:{}/{}".format(
                    self.user, self.password, self.host, self.port, self.name),
                encoding="utf-8",
                echo=False,
                pool_recycle=3600,
                pool_size=30)

    def open(self):
        try:
            self.session = sessionmaker(bind=self.engine)
            return True
        except Exception as e:
            return False
```
16. python的垃圾回收机制 -> [Python系列 - 浅析Python的垃圾回收机制](https://runnerliu.github.io/2017/07/16/pythongc/)
17. celery -> [Python库-celery](https://runnerliu.github.io/2020/11/18/python-lib-celery/)
18. tornadoweb漏洞相关
 - cookie漏洞：set_secure_cookie
 - 请求漏洞：CSRF(网站过分信任用户，放任来自所谓通过访问控制机制的代表合法用户的请求执行网站的某个特定功能，跨站请求伪造) XSS(网站过分信任用户，放任来自所谓通过访问控制机制的代表合法用户的请求执行网站的某个特定功能，跨站脚本攻击), xsrf_cookies
 - authenticated装饰器
 - sql注入、html注入
19. tornado高版本与低版本的区别
20. tornado性能强悍的原因
21. 装饰器的底层实现
 - 使用闭包，函数也是对象，可以作为参数传递

### Golang相关

1. 切片和数组的区别
2. make和new的区别，make第三个形参的意义
3. go的垃圾回收机制 -> [GO系列 - 垃圾回收机制](https://runnerliu.github.io/2021/02/19/gogc/)

### 消息队列相关

1. RocketMQ消息丢失的处理方式 -> [RocketMQ如何处理消息丢失](https://runnerliu.github.io/2021/05/18/rocketmq-msglost/)

### 网络相关

1. http三次握手，如果只有2次会出现什么问题 -> [TCP/IP协议入门篇](https://runnerliu.github.io/2017/04/03/tcpip/)
2. http协议的组成，http状态码，400和404的区别 -> [HTTP常用状态码详解](https://runnerliu.github.io/2017/04/04/httpstatecode/)
3. http与https的区别 -> [HTTP1.0、HTTP1.1、HTTP2.0的区别](https://runnerliu.github.io/2020/10/25/tcpdiff/)
4. https密钥协商过程 -> [彻底搞懂HTTPS的加密原理](https://zhuanlan.zhihu.com/p/43789231)
5. http2.0的优势 -> [HTTP1.0、HTTP1.1、HTTP2.0的区别](https://runnerliu.github.io/2020/10/25/tcpdiff/)
6. OSI模型，nginx属于应用层，webrtc协议属于应用层 -> [TCP/IP协议入门篇](https://runnerliu.github.io/2017/04/03/tcpip/)
7. ip地址的路由转换
8. 文本协议和二进制协议的区别，哪些协议属于二进制协议
9. url从发起到后端响应的整个过程 -> [从输入 URL 到页面展示到底发生了什么](https://runnerliu.github.io/2017/06/22/urlrequestprocess/)
10. websocket与socket的区别 -> [Websocket与Socket的区别](https://runnerliu.github.io/2021/05/18/websocket-socket/) [聊聊WebSocket与Socket.IO](https://runnerliu.github.io/2021/05/15/websocket-socketio/)
11. 10个请求会启动100个协程、100个请求会启动1000个协程，每个协程的处理时间会不会有影响
12. WSS与WS的区别 -> [聊聊WS和WSS](https://runnerliu.github.io/2021/02/18/wsandwss/)
13. [HTTP POST提交数据分常见方式](https://runnerliu.github.io/2018/05/13/httppostmethod/)
14. [阻塞|非阻塞 - 异步|同步](https://runnerliu.github.io/2017/07/20/zsfzsybtb/)
15. 并行和并发 -> [并发与并行的区别是什么？](https://www.zhihu.com/question/33515481)
 - 并发是一个处理器同时处理多个任务，并行是多个处理器或者是多核的处理器同时处理多个不同的任务
16. traceroute实现原理

### 缓存相关

1. redis的数据类型，各数据类型的底层实现方式
2. redis的集合、有序集合的查找复杂度
3. redis抗住高并发的原理
4. redis主从同步，主从切换
5. 缓存雪崩、缓存穿透、缓存击穿 -> [Redis系列 - 缓存雪崩、击穿、穿透、预热、更新](https://runnerliu.github.io/2021/05/23/redis-cachedown/)
6. [Redis系列 - 分布式锁](https://runnerliu.github.io/2018/05/06/distlock/)
7. redis连接池的性能提升有多大

### 数据库相关

1. mysql的数据类型，各数据类型存储字节、大小 -> [Mysql系列 - 数据类型](https://runnerliu.github.io/2021/06/29/mysql-datatype/)
2. mysql聚集索引、非聚集索引
3. mysql为什么用B+树，与使用hash的区别
4. 事物的隔离级别
5. mysql主从同步，主从切换
6. mysql与mongodb的区别
7. 回表查询 -> [Mysql系列 - 回表查询和覆盖索引](https://runnerliu.github.io/2021/05/23/mysql-huibiao/)
8. 慢查询，如何解决
9. mysql如果未响应如何做
10. mysql的建表、更新、设置索引语句
11. s_id c_id score，写sql统计每门课程的最大分数、每门课程的平均分、sql的优化，group by是否用索引
```
SELECT l_id, MAX(score) AS maxScore FROM lessons GROUP BY 

SELECT l_id, sumScore/countStu AS avgScore FROM (SELECT l_id, SUM(score) AS sumScore, COUNT(s_id) AS countStu FROM lessons GROUP BY l_id) tmp

SELECT l_id, AVG(score) AS avgScore FROM lessons GROUP BY l_id

group by 先排序再分组
```
12. [Mysql系列 - 事务](https://runnerliu.github.io/2017/08/28/mysqltransaction/)
13. [Mysql系列 - InnoDB与MyISAM](https://runnerliu.github.io/2017/07/15/myisaminnodb/)
14. [Mysql系列 - UNIQUE KEY与PRIMARY KEY](https://runnerliu.github.io/2017/07/15/uniqueprimarykey/)
15. mysql实现分页查询

### 操作系统相关

1. 进程、线程、协程区别，各自的使用场景，优势缺点 -> [子进程和线程的区别](https://runnerliu.github.io/2017/07/15/threadsubprocess/) [进程与线程的关系](https://runnerliu.github.io/2017/04/04/processthread/)
2. 进程间通信方式
3. 什么是系统调用
4. 什么是io多路复用 -> [I/O多路复用](https://runnerliu.github.io/2021/06/21/io-multiplexing/)
5. 线程1:1 1：n模型，应用程序的线程与内核线程如何对应
6. 用户态和内核态 -> [Linux的用户态和内核态](https://runnerliu.github.io/2017/07/16/linuxyhtnht/)
7. 死锁
8. 僵尸进程和孤儿进程
 - 僵尸进程：当进程exit()退出之后，他的父进程没有通过wait()系统调用回收他的进程描述符的信息，该进程会继续停留在系统的进程表中，占用内核资源，这样的进程就是僵尸进程。
 - 孤儿进程：当一个进程正在运行时，他的父进程忽然退出，此时该进程就是一个孤儿进程。作为一个进程，需要找到一个父进程，否则这种进程在退出之后没人回收他的进程描述符，空耗内存。此时该进程会找到一个父进程，如果自己所在的进程组没人收养，那就作为init进程的子进程。

### 数据结构相关

1. hash表的实现，hash冲突，hash冲突如果很多怎么解决
 - hash冲突：开放地址法和链表法
2. 链表查找如何更快
 - [跳表](https://segmentfault.com/a/1190000039386299)
3. 数组和链表的区别
 - 都属于线性表：所有数据都排列在只有一个维度的“线”上
 - 数组在物理内存上是连续存储的，支持随机访问，而链表在物理内存上一般是不连续的，支持顺序访问，查找复杂度为O(1)、O(n)
 - 插入复杂度：数组O(n)，链表O(1)
 - 删除复杂度：数组O(n)，链表O(1)
4. 常见的数据结构
 - 栈、队列、数组、链表、树、图、堆、散列表

### 设计模式相关

1. 常用的设计模式及基本思想
 - 单例模式、观察者模式、工厂模式、策略模式、适配器模式
2. [单例模式](https://runnerliu.github.io/2017/08/27/singleinstancepython/)

### 架构设计相关

1. 秒杀系统的设计
 - 限流、预热、隔离、削峰、一致、兜底
 - [秒杀系统设计01 - 秒杀系统架构设计都有哪些关键点？](https://runnerliu.github.io/2021/07/04/miaosha01/#more)
2. 问卷系统的设计
3. 直播系统架构可优化的地方
 - 延时、webrtc架构、自动巡检、半自动修复
4. 分布式一致性
5. 负载均衡的部署
6. 红绿灯的调度设计

### 算法相关

1. 二叉树层序遍历，返回二维数组 -> [二叉树的层序遍历](https://github.com/runnerliu/leetcode-hot-100#102-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86)
2. func_a返回随机1-7 利用func_a实现func_b返回随机1-5
 - 可以直接调用rand7()，如果生成的数字大于5，再次调用
 - 引申 -> [470. 用 Rand7() 实现 Rand10()](https://github.com/runnerliu/leetcode-hot-100#470-%E7%94%A8-rand7-%E5%AE%9E%E7%8E%B0-rand10)
3. 14e个数字，找到第1e大的数 -> [数组中的第k个最大元素](https://github.com/runnerliu/leetcode-hot-100#215-%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E7%AC%ACk%E4%B8%AA%E6%9C%80%E5%A4%A7%E5%85%83%E7%B4%A0)
 - 利用快排的分治思想
4. 二叉树S形遍历 -> [二叉树的锯齿形层序遍历](https://github.com/runnerliu/leetcode-hot-100#103-%E4%BA%8C%E5%8F%89%E6%A0%91%E7%9A%84%E9%94%AF%E9%BD%BF%E5%BD%A2%E5%B1%82%E5%BA%8F%E9%81%8D%E5%8E%86)
5. 一条曲线，获取以10min为长度的最大值，避免每次重复计算
6. 检测链表有环 -> [环形链表](https://github.com/runnerliu/leetcode-hot-100#141-%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8)
7. 1w的阶乘有多少个0 -> [阶乘后的零](https://github.com/runnerliu/leetcode-hot-100#172-%E9%98%B6%E4%B9%98%E5%90%8E%E7%9A%84%E9%9B%B6)
8. 孤岛问题，0 1的矩阵，上下左右能相连的1是一个岛，数出来一个矩阵中有多少个孤岛 -> [统计封闭岛屿的数目](https://github.com/runnerliu/leetcode-hot-100#1254-%E7%BB%9F%E8%AE%A1%E5%B0%81%E9%97%AD%E5%B2%9B%E5%B1%BF%E7%9A%84%E6%95%B0%E7%9B%AE)
9. 长url如何转换成短url
 - 事先生成n个短链接，将短长链接的对应关系存储在数据库中
10. str_1 = "helloworld"、str_2 = "oworldhell"，func(str_1, str_2)  ->  true / false
```
def reverse(str1, str2) -> bool:
    if not str2 or not str2:
        return False
    key = str2[0]
    l1 = len(str1)
    l2 = len(str2)
    for i in range(l1):
        if str1[i] == key:
            if str1[i:] == str2[:(l1 - i)] and str1[:i] == str2[(l1 - i):]:
                return True
    return False
```
11. 一个数组，包含0和非0的整数，将0转移到前边，其他非0依次移动但前后顺序不变 -> [移动零](https://github.com/runnerliu/leetcode-hot-100#283-%E7%A7%BB%E5%8A%A8%E9%9B%B6)
12. 二分查找
13. 兄弟词
14. 整数数组nums，目标值target，从数组中找到和是target的两个数，返回他们的索引，假设有且只有一种解 -> [两数之和](https://github.com/runnerliu/leetcode-hot-100#1-%E4%B8%A4%E6%95%B0%E4%B9%8B%E5%92%8C)
15. 链表相加 -> [两数相加](https://github.com/runnerliu/leetcode-hot-100#2-%E4%B8%A4%E6%95%B0%E7%9B%B8%E5%8A%A0)
16. 大整数list获取topn和最大值 -> [数组中的第k个最大元素](https://github.com/runnerliu/leetcode-hot-100#215-%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E7%AC%ACk%E4%B8%AA%E6%9C%80%E5%A4%A7%E5%85%83%E7%B4%A0)
17. [排序算法总结](https://runnerliu.github.io/2017/04/11/phpsortalgorithm/)
18. 如何实现一个定时器 -> [构建企业级业务高可用的延时消息中台](https://club.perfma.com/article/641128)
 - 排序双向链表
 - 小顶堆
 - 时间轮
 - 多层时间轮
19. 实现LRU算法
```
class LRUCache(object):

    def __init__(self, cap):
        self.cap = cap
        self.cache = {}
        self.keys = []

    def get(self, key):
        if key in self.cache:
            value = self.cache[key]
            self.keys.remove(key)
            self.keys.insert(0, key)
        else:
            value = -1
        return value

    def put(self, key, value):
        if key in self.cache:
            self.keys.remove(key)
        elif len(self.keys) == self.cap:
            old_key = self.keys.pop()
            del self.cache[old_key]
        self.keys.insert(0, key)
        self.cache[key] = value
```
20. 合并N个有序数组
```
import heapq
from collections import deque

def list_merge(*lists):
    #入参判断, 这里直接pass
    #将所有链表转化为deque,方便使用popleft获取链表的最左元素及根据索引返回该索引对应的剩余链表
    queues = [queue for queue in map(deque, lists)]
    heap = []
    #初始化链表,该链表中的元素为元组, 各个链表的第一个元素及链表所在索引
    for i, lst in enumerate(queues):
        heap.append((lst.popleft(), i))
    #将链表转换成最小堆
    heapq.heapify(heap)
    #链表: 用于存放每次获取的堆顶层元素
    result = []
    
    while heap:
        #将堆顶层元素出堆
        value, index = heapq.heappop(heap)
        #将顶层元素追加
        result.append(value)
        #根据索引获取对应链表的剩余元素
        if queues[index]:
             #如果存在下一个元素,则将该元素及索引入堆
            heapq.heappush(heap, (queues[index].popleft(), index))
    return result
```

### 其他
1. 为何选择看机会
2. 行业选择，职业规划及方向
3. 项目的难点，从项目中学习到了什么，项目的设计思想