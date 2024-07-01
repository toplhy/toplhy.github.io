```
// 关闭防火墙
systemctl stop firewalld.service
// 开启防火墙
systemctl start firewalld.service
// 查看防火墙状态
systemctl status firewalld.service
//查看端口开放情况
sudo firewall-cmd --list-all

// 开放http 80端口
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-port=80/tcp --permanent
//删除80端口
sudo firewall-cmd --remove-port=80/tcp --permanent

//重启防火墙
sudo firewall-cmd --reload
```
