---
title: "Ansible playbook : few tips"
date: 2020-05-21 21:30:30 +0900
categories: Knowledges
comments: true
---

I had to collect few informations from thousands of different client servers. Good thing was that I had all the credentials, so I took this opportunity to use Ansible and batch all shell script with ansible playbook.

I found this job quite easy, and really handy for batch jobs without any remote agents or whole automation system to control credentials. I'll share those small tips that I found very useful while this job.

## Full-scale shell scripts

```yaml
---
- hosts: all
  gather_facts: no
  vars:  
    host_name: all  
  tasks:  
  - name: log check  
    shell: |
      WEB_SV='nginx'

...and so on...
```

To all hosts registered in Ansible, I wanted to check if any of nginx logs contain 5XX Error which can be a failure of our application. Not going deep into it, I just want to scan all logs and find if any of logs contains response code that starts with 5.

You can write a complete shell script in playbook, as long as you follow the yaml indentation. You can leave blank line for better readability too. This kind of automation allows you to execute a batch shell over thousands of hosts easily.

## Debug as a stdout platform

The problem of these batch process might be the control of results. If there are some critical erros in web server, how do I detect those result from shell script? I found a very simple answer from [stackoverflow][tip], that you can print stdout result of each host in debug.

```yaml
...
  tasks:  
  - name: log check  
    shell: |

...shell scripts...

      done
      echo $((`date "+%s"` - UNIX_T )) 'seconds.'
    become: true
    register: log
  - debug: var=log.stdout_lines
```

Key is tasks' register and debug parameter. When I register whole task with certain variable, I can print any dictionary value inside of that variable during debug process:

```shell
[root@test-vm ansible-playbook]# ansible-playbook log_check.yaml

PLAY [all] *************************

TASK [log check] *******************
changed: [40.111.111.111]
changed: [20.111.111.111]

TASK [debug] ***********************
ok: [40.111.111.111] => {
    "log.stdout_lines": [
       "/home/test1/test1 : NO ERROR.",
        "0 seconds."
    ]
}
ok: [20.111.111.111] => {
    "log.stdout_lines": [
       "/home/test2/test2 : NO ERROR.",
        "/home/test3/test3 : NO ERROR.",
        "/home/test4/test4 : NO ERROR.",
        "0 seconds."
    ]
}

PLAY RECAP *************************
40.111.111.111              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
20.111.111.111             : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

```

You get TASK [debug] result from ansible-playbook process, with all the stdout that was produced while my shell script into list. Since then, with few string search and modification, you can get a pure data of every hosts' results.

Simple tip, but very powerful. nowadays any automation tools try to provide lots of prefixed options with UI, but if you're OG that stick with CLI and scripts, ansible gives you wider freedom.

[tip]: https://stackoverflow.com/questions/20563639/ansible-playbook-shell-output
