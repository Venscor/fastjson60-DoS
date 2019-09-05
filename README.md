# fastjson 1.2.60以下 远程DOS

## 如何复现

重复发送请求，一直到内存打满

（1）发送单个请求，会导致当前线程OOM，导致当前线程无法申请更多的内存。并且，由于异样没有被捕获，导致Server 异常，出现500错误，当前线程crash掉。
    参考：https://www.xttblog.com/?p=3097
    https://www.zhihu.com/question/33194730
    https://blog.csdn.net/SakuraInLuoJia/article/details/89502822
（2）tomcat 对servlet有关，线程出现OOM时，出现问题的线程内存并没有得到有效回收。下次请求时，再次申请内存，最终导致程序内存一直增长，最终导致DoS。

## 启发

其实这么看，其实也有可能直接出发业务的DoS。例如某个请求包返回了5xx，并且response出现了"Java heap space"特征。说明可能出发了业务的OOM，
这样可能也会导致DoS。

