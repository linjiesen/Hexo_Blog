---
title: Hexo-Deploy-ssh-Permission-denied
date: 2019-03-19 17:51:00
tags: [Hexo,Github]
categories: Hexo
---

# 问题描述:使用hexo发布博客,配置完成之后执行 `hexo g -d` 出现如下错误
```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: Spawn failed
    at ChildProcess.<anonymous> (/home/alroy/Github/hexo-next/node_modules/hexo-util/lib/spawn.js:52:19)
    at emitTwo (events.js:126:13)
    at ChildProcess.emit (events.js:214:7)
    at Process.ChildProcess._handle.onexit (internal/child_process.js:198:12)
```

简单Google一下之后,发现是Github上没有密钥的原因
\> ls ~/.ssh/
\> ssh-keygen -t rsa -C “youremail”
\> cat ~/.ssh/id\_rsa.pub

登录github后，进入个人设置settings---\>ssh and gpg keys--\>new ssh key 添加即可。title自行命名
