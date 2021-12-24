# Homestead
``` shell script
# 刷新配置文件
cd ~/Homestead && vagrant provision && vagrant reload
# 编辑配置
atom /Users/libin/Homestead/Homestead.yaml
# 查看已有的box
vagrant box list
# host添加
echo "192.168.56.56   fqj.test" | sudo tee -a /etc/hosts
find / -iname nginx
sudo /usr/sbin/nginx -s reload
# nginx日志
/var/log/nginx/
# nginx conf目录
/etc/nginx/sites-available/
```

## Redis相关
```shell script
# 设置为开机自启动服务器
chkconfig redisd on
# 打开服务
service redisd start
# 关闭服务
service redisd stop
# 命令行
redis-cli
config set requirepass uNn~HqBo&T[qT3F9s2

sudo vi /etc/redis/redis.conf
protected-mode yes 改成 protected-mode no
添加 requirepass xxx xxx 为设定的密码
注释掉 bind 127.0.0.1
sudo /etc/init.d/redis-server restart

```

# 下离线包
```shell script
# 下载地址：
https://app.vagrantup.com/laravel/boxes/homestead


{
    "name": "laravel/homestead",
    "versions": [{
        "version": "11.4.0",
        "providers": [{
            "name": "virtualbox",
            "url": "file:///Users/libin/Homestead/2ba89637-7029-4a72-a37d-0a722990de45"
        }]
    }]
}


vagrant box add metadata.json
```
