# domain convergence

## domain convergence
PC 时代为了突破并发请求，采用了多域名的方法，让浏览器可以在同一时间并发请求多个域名下的资源，但是在移动 web 中，这样可能性能就有问题了，因为移动端的网速远远不及 PC 端的有线，而且 DNS 解析在弱网环境是比较慢的， DNS 解析路径如下：
```
root
|--com
|    |--yy
|    |   |--static
|    |   |--page
|    |   |--www
|    |
|    |--duowan
|    |    |--stitic
|    |    |--wap
|
|--net
|   |---yy
|   |    |---static
|   |    |---wap
|   |    |---www
|   |---duowan
|   |    |---static
|   |    |---wap
|--org
|   |---yy
|   |    |---static
|   |    |---wap
|   |    |---www
|   |---duowan
|   |    |---static
|   |    |---wap
```
其实 www.yy.com 这样的 url 完整的是 www.yy.com. 那个 . 就是 root 根目录，只不过平时浏览器帮我们补齐了。查找域名的顺序就是按照上面的目录的顺序来的，先 com 然后 yy 然后是 www。
计算机网络里面有说全世界根域有 13 个，也就是 root 有 13 个。NameServer 就是权威服务器，就是某些域的权威， com、net、org 的权威就是上面的 root。也就是 yy.com 的权威是 com 所在的 NS，www.yy.com  的权威是 yy.com 所在的 NS。
就是因为这么深的查找路径，而且在无线的弱网环境下，所以才显得慢，为了减少开销，可以减少 DNS 请求，也可以缩短 DNS 解析路径。
第一个就是做域名收敛的原因，将 CDN 相关静态资源集合到一个域名下，加快解析速度，第二个可以采用 HTTPDNS 服务，通过 http 请求拿到域名服务器地址。
