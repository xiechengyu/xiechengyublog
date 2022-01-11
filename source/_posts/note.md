```js
//语法:
lsof -i[46] [protocol][@hostname|hostaddr][:service|port]
lsof -i //获取所有连接
```

46 --> IPv4 or IPv6

protocol --> TCP or UDP

hostname --> Internet host name

hostaddr --> IPv4位置

service --> /etc/service中的 service name (可以不只一个)

port --> 端口号 (可以不只一个)





