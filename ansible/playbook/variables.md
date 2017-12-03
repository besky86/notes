# 变量

ansible中的变量包含英文字母、数字和下划线，以字母开始.

定义map类型变量后引用，请使用arg['key']格式

##  变量的优先级, 依次上升：
* role defaults [1]
* inventory file or script group vars [2]
* inventory group_vars/all
* playbook group_vars/all
* inventory group_vars/*
* playbook group_vars/*
* inventory file or script host vars [2]
* inventory host_vars/*
* playbook host_vars/*
* host facts
* play vars
* play vars_prompt
* play vars_files
* role vars (defined in role/vars/main.yml)
* block vars (only for tasks in block)
* task vars (only for the task)
* role (and include_role) params
* include params
* include_vars
* set_facts / registered vars
* extra vars (always win precedence)

## 变量的范围

Global: this is set by config, environment variables and the command line
Play: each play and contained structures, vars entries (vars; vars_files; vars_prompt), role defaults and vars.
Host: variables directly associated to a host, like inventory, include_vars, facts or registered task outputs


Child groups override parent groups, and hosts always override their groups.
