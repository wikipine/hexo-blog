---
title: Hexo 图片展示
date: 2019-05-09 21:36
categories: ['前端']
tags: ['hexo']
comments: true
---

# 题记

本来是传到简书上做图库，不太道德，而且简书做了防盗链的处理，那流量就是从自己本地走好了。

转载 [优化hexo目录，使本地图片能显示出来](https://www.cnblogs.com/chanchifeng/p/9253078.html)


1. 修改配置文件_config.yml 里的post_asset_folder:这个选项设置为true

2. 在你的hexo目录下执行这样一句话npm install hexo-asset-image --save，这是下载安装一个可以上传本地图片的插件，来自dalao：dalao的git（未验证有什么用）

3. 等待一小段时间后，再运行hexo n "xxxx"来生成md博文时，/source/_posts文件夹内除了xxxx.md文件还有一个同名的文件夹

4. 最后在xxxx.md中想引入图片时，先把图片复制到xxxx这个文件夹中，然后只需要在xxxx.md中按照markdown的格式引入图片：
        ![你想输入的替代文字](xxxx/图片名.jpg)
        
       
# END

图片暂时也不算多，也不太喜欢传图，后期应该会把本博客的图片转到腾讯云的COS 上，免费空间挺大的。