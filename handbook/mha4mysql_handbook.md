# [MHA](https://github.com/yoshinorim/mha4mysql-manager/wiki)

## [Node](https://github.com/yoshinorim/mha4mysql-node)

### [Installation](https://github.com/yoshinorim/mha4mysql-manager/wiki/Installation#Installing_MHA_Node)

Ubuntu

```
sudo apt install make
sudo apt-get install libmodule-install-perl
sudo apt-get install libdbd-mysql-perl

perl Makefile.PL
make
sudo make install

ls -l /usr/local/bin
```



```
apply_diff_relay_logs
filter_mysqlbinlog
purge_relay_logs
save_binary_logs
```

## [Manager](https://github.com/yoshinorim/mha4mysql-manager)

### [Installation](https://github.com/yoshinorim/mha4mysql-manager/wiki/Installation#Installing_MHA_Node)

Ubuntu

```
#sudo apt-get install libdbd-mysql-perl
sudo apt-get install libconfig-tiny-perl
sudo apt-get install liblog-dispatch-perl
sudo apt-get install libparallel-forkmanager-perl

perl Makefile.PL
make
sudo make install

ls -l /usr/local/bin
```



```
apply_diff_relay_logs
filter_mysqlbinlog
masterha_check_repl
masterha_check_ssh
masterha_check_status
masterha_conf_host
masterha_manager
masterha_master_monitor
masterha_master_switch
masterha_secondary_check
masterha_stop
purge_relay_logs
save_binary_logs
```

### Configuration

```
[server default]
ssh_user=alvin

# mysql user and password
user=root
password=root
# working directory on the manager
manager_workdir=/var/log/masterha/app1
# manager log file
manager_log=/var/log/masterha/app1/app1.log
# working directory on MySQL servers
remote_workdir=/var/log/masterha/app1

ping_interval=3

master_binlog_dir= /var/lib/mysql
#master_ip_failover_script=/script/masterha/master_ip_failover

#secondary_check_script= masterha_secondary_check -s remote_host1 -s remote_host2
#shutdown_script= /script/masterha/power_manager
#report_script= /script/masterha/send_master_failover_mail

[server1]
hostname=192.168.1.1

[server2]
hostname=192.168.1.2
candidate_master=1

[server3]
hostname=192.168.1.3
```

