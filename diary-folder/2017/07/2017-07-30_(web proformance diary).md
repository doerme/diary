# web proformance diary

其实像 webpagetest 这样的性能监测工具, 其实使用的原理就是使用系统底层 API 监听 TCP/IP 的接收数据, 监测网页性能, 但是像现在就不需要了, 很多浏览器都提供了 performance 
API , 可以提供给我们查询网页性能信息的.

localstorage 的读写性能并没有这么好, 它是将数据写到了磁盘中的, 磁盘的读写代价是比较大的, 所以优化 localstorage 的性能是将键值对的值尽量地多写, 举个例子, 读一次 10000 条的数据
比分 10 次读 1000 条数据的性能要好.

内联不一定可以加快网页速度, CDN 可以被缓存, 内联因为需要从服务器上获取, 所以会稍微减慢了网页加载的速度.
脚本,图片懒加载可能会加快页面的首屏加速.比较好的 TTFB 是 100ms

我们知道性能监测是重要的, 但是有可能会出现错误的判断, 因为样本量不足, 可能会出现错误的判断.
做性能监测系统, 最重要的是直方图, 可以看到性能结果的分布, 分析一个数据最好使用中值, 而不是使用均值.

浏览器会使用样式共享, 也就是同一类的元素的样式会相同, 不会重复计算.并且使用规则哈希, 也就是说将选择器进行分组, 减少了查找元素的工作.而且使用父过滤器或者快速路径,但是为什么还是慢? 是因为直接或者相邻的相邻选择器性能还是很差, 尽管快速路径优化了相当大一部分的后代, 子, 标签, ID, 类, 属性选择器的性能.