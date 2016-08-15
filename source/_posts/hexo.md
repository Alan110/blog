---
title: 手把手教你用hexo + github 搭建静态博客
date: 2016-08-13 13:58:13
tags:
---

本来已经有了博客园的博客,但是觉得排版太麻烦,而且受制于平台,不能随性迁移.碰巧看到同事自己搭建的个人网站,心痒痒的.有自己的个人网站是多么炫酷的事情.于是我也去了解了一些建站的相关知识.阿里云主机也去弄了个来玩儿玩儿,不过1月后就没有再续费了.原因嘛很简单,从头到尾自己搞.成本太高了.虽然说能锻炼自己的个人能力,那是肯定的.但是前期投入也确实太大了.本来业余时间也没多少,不想那么瞎折腾.只想找个地儿,简单写写心得体会.本文便是我搭建的全过程.


## 介绍

最近比较流行静态博客网站.即站点并不是动态语言生成,而是静态html页面. hexo就是一个基于markdown的nodejs平台的html静态博客生成器.据说是个台湾人写的.同类型的有jekyll,这玩意儿是基于ruby的,本人不熟.而且gem速度慢的要死,国内被墙了,你懂的.身为前端开发者,果断拥抱nodejs.markdown就不说了,不懂的请自行百度.

## 安装

1. 安装依赖

    ```
        $ sudo brew install nodejs;
        $ sudo brew install git;
    ```

2. 安装hexo

    ```shell
        $ sudo npm install -g hexo-cli
    ```

## 简单使用

* 初始化一个工程目录

    ```
        $ hexo init <folder>
        $ cd <folder>
        $ npm install
    ```

* 目录介绍

    ```
        .
        ├── _config.yml         // 全局配置
        ├── package.json
        ├── scaffolds           // 存放草稿
        ├── source              // 源码目录
        |   ├── _drafts         // 草稿文件
        |   └── _posts          // 发布的文件
        └── themes              // 主题文件存放地
    ```




* 创建一篇文章

    ```
    & hexo new [layout] <title>

    ```
    如果layout没有传入, 会使用_config.yml文件中的default_layout字段,默认是post
    执行命令后,会自动创建一个md文件到/source/_posts/目录下.你可以用你喜欢的编辑器进行编写了.



* 本地预览

    ```
    $ hexo server
    ```

## 远程部署到github

>利用github的page功能,可以部署静态网页,拥有无限流量.我们完全可以通过此功能打造我们自己的静态博客网站.不用花一分钱.

1. 首先在github上建立一个Repository,仓库名必须是your_user_name.github.io

2. 设置ssh公钥通过验证.

    ```
        $ ssh-keygen -t rsa -C "your_email@example.com" 
    ```
    运行此命令会根据你提供的邮箱创建一对秘钥.

    拷贝.ssh/目录下的id_rsa.pub中的公钥到github

    ![](http://o99eh3ii0.bkt.clouddn.com//16-8-13/74961271.jpg)

    验证是否联通成功
    ```
        ssh -T git@github.com
    ```
    如果成功了,会出现
    ![](http://o99eh3ii0.bkt.clouddn.com//16-8-13/40882604.jpg)

3. 然后配置hexo关联github,修改_config.yml的deploy属性如下:

    ```
        deplop:

             type: git

             repo: git@github.com:Alan110/Alan110.github.io.git(你的仓库地址)

             branch: master
    ```

    安装hexo-deploy-git插件
    ```
        npm install hexo-deployer-git --save
    ```

3. 写好博客后,build下,然后发布. 生成的html静态文件就会发布到github上

    ```
        $ hexo generate (缩写g) 

        $ hexo deploy (缩写d)
    ```

4. 访问你的博客

   github page页面的访问路径是your_username.github.io

## github.io关联域名

>我们可以给page页面关联上自己的域名,这样它看起来就非常像我们的个人网站了.

* 购买域名

    我是在万网购买的,便宜的1年也就45元.

    配置域名解析
    ![](http://o99eh3ii0.bkt.clouddn.com//16-8-14/65138860.jpg)

* github配置CNAME

    在项目根目录下添加CNAME文件,内容写上域名,注意没有http等协议.

    hexo会覆盖式同步整个工程目录到github,所以CNAME文件要放在source目录里面,否则下次同步会被覆盖.


## 结语

>好了,我们已经完成了一个静态博客网站.可以开始发散我们的思维,记录生活,专研技术.to be better for your love !! .

>hexo挺强大的,有很多自定义配置,有兴趣的同学和看看我的另一篇文章

