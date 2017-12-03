# Role

## 目录结构：
```
site.yml
webservers.yml
fooservers.yml
roles/
   common/
     tasks/
     handlers/
     files/
     templates/
     vars/
     defaults/
     meta/
   webservers/
     tasks/
     defaults/
     meta/
```
role的文件夹下面，每个(如tasks)文件夹必须有一个`main.yml`文件

文件夹说明：
* tasks - contains the main list of tasks to be executed by the role.
* handlers - contains handlers, which may be used by this role or even anywhere outside this role.
* defaults - default variables for the role (see Variables for more information).
* vars - other variables for the role (see Variables for more information).
* files - contains files which can be deployed via this role.
* templates - contains templates which can be deployed via this role.
* meta - defines some meta data for this role. See below for more details.

目录引用示例：
```
# roles/example/tasks/main.yml
- name: added in 2.4, previouslly you used 'include'
  import_tasks: redhat.yml
  when: ansible_os_platform|lower == 'redhat'
- import_tasks: debian.yml
  when: ansible_os_platform|lower == 'debian'

# roles/example/tasks/redhat.yml
- yum:
    name: "httpd"
    state: present

# roles/example/tasks/debian.yml
- apt:
    name: "apache2"
    state: present
```

## 使用Role
基本用法是在play中使用`roles:`选项
```
---
- hosts: webservers
  roles:
     - common
     - webservers
```

Role中的引用：
* 如果 roles/x/tasks/main.yml 存在, 其中列出的 tasks 将被添加到 play 中
* 如果 roles/x/handlers/main.yml 存在, 其中列出的 handlers 将被添加到 play 中
* 如果 roles/x/vars/main.yml 存在, 其中列出的 variables 将被添加到 play 中
* 如果 roles/x/meta/main.yml 存在, 其中列出的 “角色依赖” 将被添加到 roles 列表中 (1.3 and later)
* 所有 copy tasks 可以引用 roles/x/files/ 中的文件，不需要指明文件的路径。
* 所有 script tasks 可以引用 roles/x/files/ 中的脚本，不需要指明文件的路径。
* 所有 template tasks 可以引用 roles/x/templates/ 中的文件，不需要指明文件的路径。
* 所有 include tasks 可以引用 roles/x/tasks/ 中的文件，不需要指明文件的路径。


## playbook的执行顺序：
1. 任何在play中的pre_tasks, Any pre_tasks defined in the play.
2. 被pre_task出发的Handlers,Any handlers triggered so far will be run.
3. 每一个roles依次执行，在每一个roles执行前都会执行其依赖,Each role listed in roles will execute in turn. Any role dependencies defined in the roles meta/main.yml will be run first, subject to tag filtering and conditionals.
4. play中定义个所有tasks,Any tasks defined in the play.
5. 所有被出发的handlers, Any handlers triggered so far will be run.
6. 所有post_tasks, Any post_tasks defined in the play.
7. 所有出发的handlers,Any handlers triggered so far will be run.

其实可以这么理解， task的执行，包含了前置本身，handlers, 是一个整体，依次执行,记为task整体,
那么playbook的执行就是：前置task整体执行，每一个role整体执行，所有后置整体执行。

## 在task中包含roles:
```
---

- hosts: webservers
  tasks:
  - debug:
      msg: "before we run our role"
  - import_role:
      name: example
  - include_role:
      name: example
  - debug:
      msg: "after we ran our role"
```

## 指定role路径的方式：
```
---

- hosts: webservers
  roles:
    - { role: '/path/to/my/roles/common' }

---
# or
- hosts: webservers
  roles:
    - common
    - { role: foo_app_instance, dir: '/opt/a', app_port: 5000 }
    - { role: foo_app_instance, dir: '/opt/b', app_port: 5001 }

# or

---

- hosts: webservers
  tasks:
  - include_role:
       name: foo_app_instance
    vars:
      dir: '/opt/a'
      app_port: 5000
  ..
```

## 相同的role执行

如果role引用多次，但是只会执行依次：
```
---
- hosts: webservers
  roles:
  - foo
  - foo
```

如果想执行多次，有以下办法：
* 不同的参数
```
---
- hosts: webservers
  roles:
  - { role: foo, message: "first" }
  - { role: foo, message: "second" }
```

* 在meta/main.yml文件中加入参数：`allow_duplicates: true`
```
# playbook.yml
---
- hosts: webservers
  roles:
  - foo
  - foo

# roles/foo/meta/main.yml
---
allow_duplicates: true
```


## role dependencies:

定义在`meta/main.yml`中
```
---
dependencies:
  - { role: common, some_parameter: 3 }
  - { role: apache, apache_port: 80 }
  - { role: postgres, dbname: blarg, other_parameter: 12 }
```
遵循重复只执行一次规则


# role搜索目录：
* A roles/ directory, relative to the playbook file.
* By default, in /etc/ansible/roles
