# 基礎配置
# 必須配置Log rotate
## nginx
```plaintext
/home/dev/webapp/docker-php/nginx/log/*.log {
  daily
  missingok
  rotate 14
  dateext
  compress
  create 644 root root
  su root root
  delaycompress
  notifempty
  sharedscripts
  postrotate
    cd /home/dev/webapp/docker-php \
      && /usr//bin/docker-compose kill -s USR1 nginx
  endscript
}
```
