---
title: Why To Use SSH ProxyCommand
description: SSH forwarding is great, but ProxyCommand lets you actually script how to make multiple jumps to get to a box.
layout: layouts/base.njk
date: 2019-09-14
tags:
  - posts
---
# You Should Switch to ProxyCommmand With SSH

SSH forwarding is great, but ProxyCommand lets you actually script how to make multiple jumps to get to a box.

To set it up, do the following:

1. Copy your public key to all the places you want it

* You may want to put your public key in puppet, but iff you put your *private* key in puppet, then everyone with access to Puppet can see it, so... no
  * `mkdir .ssh`
  * `vim .ssh/authorized_keys` and paste your private key there

2. On your laptop, create a file that looks like the following file at `.ssh/config`

```shell line-numbers
Host foo
 User warmstrong
 Hostname foo.yourdomain.dev

Host automation
 User warmstrong
 Hostname automation.yourdomain.dev

Host jump #this creates a host aliased to "jump", now you can "ssh jump"
 Hostname jumpbox.dom
 User warmstrong
 ProxyCommand ssh -q -W %h:%p foo #This means "ssh to this box using foo"

Host admin.other.dom
 User warmstrong
 ProxyCommand ssh -q -W %h:%p foo #Using the Hostname(%h) and Port(%p) you're using anyway, and passing stdio directly through(-W) but hiding errors(-q)

Host uat
 User warmstrong
 Hostname uat01.dom
 ProxyCommand ssh -q -W %h:%p jump #Note that you're using the named jump box above to go here

Host logrepo
 User warmstrong
 Hostname logrepo.dom
 ProxyCommand ssh -q -W %h:%p jump #SSH will resolve everything in the chain, so it figures out you need jump, jump needs foo, and does all those sshes to get there

Host static-server
 User warmstrong
 Hostname static.yourdomain.dom
 ProxyCommand ssh -q -W %h:%p jump #Fun fact, -W means that this works for scp as well as ssh
```

3. You can now ssh directly to `uat`, `logrepo`, etc. without hitting multiple jump boxen.
