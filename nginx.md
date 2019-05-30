#### 1、nginx的log_format配置如下：
log_format main ‘remoteaddr−remote_user [timelocal]"request”’
‘statusbody_bytes_sent “httpreferer"″"http_user_agent” “upstreamresponsetime""request_time” “http_x_forwarded_for"';
从今天的nginx log文件 access.log中：

   a、列出“request_time”最大的20行？

```shell
cat access.log | awk '{arr[$4]++} END {for(i in arr) {print arr[i],$0}}' | sort -r | head -10
```

   b、列出早上10点访问量做多的20个url地址？

```shell
cat access.log| awk '/2017:16/{arr[$1]++} END{ for(i in arr) {print ar
r[i],i}}'  | sort  -rn | head -20
```

#### 2、linux的内存分配和多线程原理

[Linux内存分配](https://blog.csdn.net/haiross/article/details/38921135)

[Linux线程和进程关系](https://my.oschina.net/cnyinlinux/blog/367910)

#### 3、**linux中怎么查看系统资源占用情况**

​    top 、free、iostat、vmstat