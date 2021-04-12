# ReverseProxy 反向代理
## ReverseProxy 组件
* ReverseProxy_s 在有外网ip的vps上使用
* ReverseProxy_c 在需要调试的内网客户端上使用
## ReverseProxy_s
* ReverseProxy_s 监听外网指定端口，将该端口上所有链接和消息通过 ReverseProxy_c 主动建立的连接传递回去，传递过程中要封包指明client来源及消息类型
## ReverseProxy_c
* ReverseProxy_c 主动建立链接,并处理 ReverseProxy_s 发过来的消息，将对应消息做转发给 本地处理端口，并将本地处理端口的消息传递回去
## eg
* A 访问 ReverseProxy_s 的80端口，建立链接时， ReverseProxy_s 发送 type connect 给 ReverseProxy_c,带上 src＿ip,src_port 组成二元组区分消息链接，中间消息正常收发，连接断开时发送 disconnect 请求
* ReverseProxy_c 收到 ReverseProxy_s 的 connect 消息时，向本地进程建立连接,并转发消息，收到任意一端的disconnect请求时，断开连接