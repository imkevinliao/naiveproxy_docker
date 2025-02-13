# naiveproxy docker
naiveproxy 绑定 caddy，naiveproxy-docker 将两者构建在了一起

caddy path: /app/caddy

Caddyfile path: /app/Caddyfile

# 快速上手

必须将域名绑定好服务器，例如域名 www.example.com

由于自动签发证书，必须使用宿主机 443 和 80 端口，否则要DNS（配置繁琐），必须使用主机 443 端口，80 选配。

注意，启动后需要等待大约半分钟（申请证书），才会生效：（访问https://domain，如果完成会显示 helloworld 字符串）

复制下面的命令，替换域名后直接执行即可

方式一(只使用443端口）
```
docker run -d --restart=always \
    -p 443:443 \
    -e DOMAIN="请替换域名：www.example.com" \
    -e USER="" \
    -e PASSWORD="" \
    -e EMAIL="" \
    --name naiveproxy kevinstarry/naiveproxy
```

只使用443端口，只配置域名，其余自动生成（小白推荐）：
```
docker run -d --restart=always \
    -p 443:443 \
    -e DOMAIN="请替换域名：www.example.com" \
    --name naiveproxy kevinstarry/naiveproxy
```


方式二（使用80和443端口）：
```
docker run -d --restart=always \
    -p 80:80 -p 443:443 \
    -e DOMAIN="请替换域名：www.example.com" \
    -e USER="" \
    -e PASSWORD="" \
    -e EMAIL="" \
    --name naiveproxy kevinstarry/naiveproxy
```

# 查看用户名和密码
如果你启动时候配置了 USER 和 PASSWORD 就没必要查看了，直接使用即可
```
docker exec -it naiveproxy sh -c "cat /app/info.txt"
```

# 完全移除
所有的数据都在容器内，宿主机不挂载，保证绝对干净：
```
docker stop naiveproxy && docker rm naiveproxy && docker rmi kevinstarry/naiveproxy
```

# 参数解析
```
ENV DOMAIN=""     顾名思义，绑定到公网ip服务器的域名
ENV USER=""       可以自定义用户名
ENV PASSWORD=""   可以自定义密码
ENV EMAIL=""      Caddy自动申请域名时候使用的申请邮箱

除了 DOMAIN 是必填，其余参数都可以省略，会随机生成
```
