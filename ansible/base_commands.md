# 1. 基础命令格式
`ansible <pattern_goes_here> -m <module_name> -a <arguments>`


## 1.1 pattern_goes_here

`ansible webservers -m service -a "name=httpd state=restarted"`

### 1.1.1 通常用于指定组

### 1.1.2 逻辑运算指定组


1. `:` 或运算:  `webservers:dbservers` 指定webservers组和dbservers组。
2. `:!` 非运算, `webservers:!phoenix`
3. `:&` 与运算, `webservers:&staging`

### 1.1.3 类似python的分割运算：
```
[webservers]
cobweb
webbing
weber
```

```
webservers[0]       # == cobweb
webservers[-1]      # == weber
webservers[0:1]     # == webservers[0],webservers[1]
                    # == cobweb,webbing
webservers[1:]      # == webbing,weber
```


### 1.1.4 正则表达式
```
~(web|db).*\.example\.com
```


# 2. Ad-Hoc 命令

## 2.1 什么是Ad-Hoc命令
> An ad-hoc command is something that you might type in to do something really quick, but don’t want to save for later.

在Ansible中，简单命令用Ad-Hoc命令即可，复杂任务执行用playbook.


## 2.2 多台节点主机并行执行shell命令
使用 -f 参数，指定并行进程数：
`ansible atlanta -a "/sbin/reboot" -f 10`


> 使用 Ansible ad hoc 命令行接口时(与使用 Playbooks 的情况相反),尤其注意 shell 引号的规则. 比如在上面的例子中,如果使用双引号”echo $TERM”,会求出TERM变量在当前系统的值,而我们实际希望的是把这个命令传递 到其它机器执行. **


## 2.3 文件操作
1. cp 文件：
```
$ ansible atlanta -m copy -a "src=/etc/hosts dest=/tmp/hosts"
```

2. 修改文件权限、所有者：
```
$ ansible webservers -m file -a "dest=/srv/foo/a.txt mode=600"
$ ansible webservers -m file -a "dest=/srv/foo/b.txt mode=600 owner=mdehaan group=mdehaan"
```

3. 创建文件夹：
```
 ansible webservers -m file -a "dest=/path/to/c mode=755 owner=mdehaan group=mdehaan state=directory"
```

4. 删除文件（夹）
 ```
 ansible webservers -m file -a "dest=/path/to/c state=absent"
 ```


从以上命令中，可以看出-m和-a是配合使用， -m是指定模块， -a指定模块执行参数

## 2.4 包管理
```
$ ansible webservers -m yum -a "name=acme state=present"
```

## 2.5 用户管理

```
$ ansible all -m user -a "name=foo password=<crypted password here>"

$ ansible all -m user -a "name=foo state=absent"
```

## 2.6 更新源码
```
ansible webservers -m git -a "repo=git://foo.example.org/repo.git dest=/srv/myapp version=HEAD"
```

## 2.7 管理服务：
```
$ ansible webservers -m service -a "name=httpd state=started"

$ ansible webservers -m service -a "name=httpd state=restarted"

$ ansible webservers -m service -a "name=httpd state=stopped"
```



引用：
>  http://docs.ansible.com/ansible/latest/intro_adhoc.html
