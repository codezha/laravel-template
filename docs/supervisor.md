```shell script
sudo vim /etc/supervisor/conf.d/horizon.conf

[program:horizon]
process_name=%(program_name)s
command=php /home/vagrant/code/项目目录名称/artisan horizon
autostart=true
autorestart=true
user=vagrant
redirect_stderr=true
stdout_logfile=/home/vagrant/code/horizon.log
stopwaitsecs=3600

sudo supervisorctl reread

sudo supervisorctl update

0.95-0.87=8



sudo supervisorctl start horizon
```
