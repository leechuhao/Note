UDP（User Datagram Protocol）用户数据包协议用于实现面向无连接和不可靠传输服务。

- 简单小巧，速度快
- 用于传输小数据流

QQ、DHCP协议、DNS协议基于UDP

常用端口号：

- 20/21：ftp
- 22：ssh
- 23：telnet
- 25：smtp
- 53：dns
- 67/68:dhcp
- 80:http
- 443:https
- 4000/8000:qq    oicq

DHCP(Dynamic Configuration Protocol)动态主机配置协议，用于动态配置IP信息（地址、网关、DNS等）

原理：

->DHCP Discover(发现)

-<DHCP offer（提供）

->DHCP request（请求）

-<DHCP ACK（确认）

question：

1. 当主机没有ip地址是，如何发DHCP包

0.0.0.0 全网地址/置空地址

2. 为什么需要4个包， 2个包不能获取ip地址吗

考虑多个DHCP服务器的环境，会造成地址浪费

​	ps：DHCP先到先得原则-谁先给offer，就向谁请求

3. 如何解决地址冲突

255.255.255.255--广播地址，解决地址冲突问题（也结合了免费ARP）