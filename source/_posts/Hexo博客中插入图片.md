---
title: Hexo博客中插入图片
date: 2019-04-06 13:45:59
tags: [Hexo]
categories: Hexo
---

# hexo博客中如何插入图片
## 1. 将根目录下的配置文件 ` _config.yml ` 中的`post_asset_folder`选项设置`true`
```
 # Writing
 42 new_post_name: :title.md # File name of new posts
 43 default_layout: post
 44 titlecase: false # Transform title into titlecase
 45 external_link: true # Open external links in new tab
 46 filename_case: 0
 47 render_drafts: false
 48 post_asset_folder: true
 49 relative_link: false
 50 future: true
 51 highlight:
 52   enable: true
 53   line_number: true
 54   auto_detect: false
 55   tab_replace:

```
## 2. 在你的hexo目录下执行这句话npm install hexo-asset-image --save，这是下载安装一个可以上传本地图片的插件：
```
(base)  alroy@Alan  ~/Github/hexo-next  sudo proxychains4 npm install hexo-asset-image --save
[sudo] password for alroy: 
[proxychains] config file found: /etc/proxychains.conf
[proxychains] preloading /usr/lib/libproxychains4.so
[proxychains] DLL init: proxychains-ng 4.13-git-10-g1198857
[proxychains] DLL init: proxychains-ng 4.13-git-10-g1198857
[proxychains] Strict chain  ...  127.0.0.1:1090  ...  registry.npmjs.org:443  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1090  ...  registry.npmjs.org:443  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1090  ...  registry.npmjs.org:443  ...  OK
[proxychains] Strict chain  ...  127.0.0.1:1090  ...  registry.npmjs.org:443  ...  OK
npm WARN babel-eslint@10.0.1 requires a peer of eslint@>= 4.12.1 but none is installed. You must install peer dependencies yourself.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.7 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.7: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

+ hexo-asset-image@0.0.3
added 13 packages from 11 contributors in 22.144s

```

## 3. 下载完成之后,下次执行 `hexo new post Hexo博客中插入图片`生成博客文章时, 会在`/source/_posts`下生成一个同名文件夹
```
(base)  alroy@Alan  ~/Github/hexo-next/source/_posts  tree
.
├── hello-world.md
├── Hexo-Deploy-ssh-Permission-denied.md
├── Hexo博客中插入图片
├── Hexo博客中插入图片.md
├── Python实现BFS和DFS.md
└── 海子逝世三十周年.md

1 directory, 5 files

```

## 4. 在文章中想引入图片时, 将图片先复制到相应的文件夹中, 再使用MarkDown语法将图片插入至文章中
> ![This is Test!](Hexo博客中插入图片/psj.jpeg)

## 5. 5 最后检查一下，hexo g生成页面后，进入public\2019\04\06\index.html文件中查看相关字段，可以发现，html标签内的语句是`<img src="2019/04/06/xxxx/图片名.jpg">`，而不是`<img src="xxxx/图片名.jpg>`。这很重要，关乎你的网页是否可以真正加载你想插入的图片。