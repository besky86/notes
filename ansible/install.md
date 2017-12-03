Ansible 的地址为 https://github.com/ansible/ansible

## Ansible 默认是通过SSH协议管理机器的
## 1. 安装

### 1.1 主机
安装使用`pip install ansible`,安装好之后需要配置节点配置文件(Inventory文件)：
##### 节点配置文件, 两个选择：
1. /etc/ansible/host
2. ~/ansible_hosts

### 1.2 托管节点
通常我们使用 ssh 与托管节点通信，默认使用 sftp.如果 sftp 不可用，可在 ansible.cfg 配置文件中配置成 scp 的方式. 在托管节点上也需要安装 Python 2.4 或以上的版本.如果版本低于 Python 2.5 ,还需要额外安装一个模块: python-simplejson


