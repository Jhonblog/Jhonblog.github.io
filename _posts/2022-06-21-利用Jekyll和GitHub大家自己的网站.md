---
layout: mypost
title: 利用Jekyll和GitHub大家自己的网站
categories: [Jekyll]
---

### 开端

偶然的机会在浏览别人的博客时，发现了一篇介绍如何搭建自己静态站点的[文章](https://winddoing.github.io/post/32555.html#%E5%8F%82%E8%80%83)，同时博主给出了参考源，文中提到可以使用Markdown编写自己的网页瞬间心动，与此同时Github的托管省去了域名和服务器维护，省时省力，于是自己开始尝试。
### 过程
由于是Windows的系统，显然要部署一下本地环境。

#### 官网流程
\
[官网](https://www.jekyll.com.cn/docs/)给出了步骤。
>```
1. 安装一个完整的[Ruby](https://www.jekyll.com.cn/docs/installation/)开发环境。
2. 安装 Jekyll 和 bundler gems。\
     `gem install jekyll bundler`
3. 在 ./myblog 目录下创建一个全新的 Jekyll 网站。\
     `jekyll new myblog`
4. 进入新创建的目录。\
     `cd myblog`
5. 构建网站并启动一个本地服务器。\
    `bundle exec jekyll serve`
6. 在浏览器中打开 http://localhost:4000 网址（或为127.0.0.1）

#### 实践

然而事情远没有想的那么简单，比如说Ruby的安装，exe安装完需要配置，而因为大陆防火墙的原因，只能开代理才能安装成功。而我自己使用的Clash就没法给CMD直接代理，需要用开代理的CMD对Ruby进行配置。而Ruby的安装脚本路径又找了半天（因为它只在安装程序后出现），最后发现 `C:\Ruby31-x64\ridk_use> .\ridk.cm` 然后，执行配置成功。
```
PS C:\Ruby31-x64\ridk_use> .\ridk.cmd
 _____       _           _____           _        _ _         ___
|  __ \     | |         |_   _|         | |      | | |       |__ \
| |__) |   _| |__  _   _  | |  _ __  ___| |_ __ _| | | ___ _ __ ) |
|  _  / | | | '_ \| | | | | | | '_ \/ __| __/ _` | | |/ _ \ '__/ /
| | \ \ |_| | |_) | |_| |_| |_| | | \__ \ || (_| | | |  __/ | / /_
|_|  \_\__,_|_.__/ \__, |_____|_| |_|___/\__\__,_|_|_|\___|_||____|
                    __/ |           _
                   |___/          _|_ _  __   | | o __  _| _     _
                                   | (_) |    |^| | | |(_|(_)\^/_>
Usage:
    C:/Ruby31-x64/bin/ridk.cmd [option]

Option:
    install                   Install MSYS2 and MINGW dev tools
    exec <command>            Execute a command within MSYS2 context
    enable                    Set environment variables for MSYS2
    disable                   Unset environment variables for MSYS2
    version                   Print RubyInstaller and MSYS2 versions
    use                       Switch to a different ruby version
    help | --help | -? | /?   Display this help and exit
```
同时还需要配置gcc和Make。这里就不多说了，注意添加环境，具体参见教程：[MinGW下载和安装教程](http://c.biancheng.net/view/8077.html)。还有`Make -v` 错误的原因可能是安装完gcc其名字叫 `mingw32-make`
```
PS C:\Ruby31-x64\bin> mingw32-make -v
GNU Make 3.82.90
Built for i686-pc-mingw32
Copyright (C) 1988-2012 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
PS C:\Ruby31-x64\bin>
```
这是Ruby的安装，接下来
```
gem install jekyll bundler
```
后面的还挺顺利的。

#### 启动
随后按着流程开始操作：
新建一个文件夹，名字随意(这里以website为例)：\
`mkdir website`\
接着新建一个工程，这里myblog是网站工程名字：（不知道这样说准确与否）\
`jekyll new myblog`\
然后进行初始化：\
`bundle install`\
最后运行网站：\
`bundle exec jekyll serve` \
第一次务必用全命令，后面可以用:\
`jekyll serve`\

#### 意外
当运行最后一条指令时往往会出错：
```
PS D:\website\myblog> bundle exec jekyll serve
Configuration file: D:/website/myblog/_config.yml
            Source: D:/website/myblog
       Destination: D:/website/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 1.18 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/website/myblog'
                    ------------------------------------------------
      Jekyll 4.2.2   Please append `--trace` to the `serve` command
                     for any additional information or backtrace.
                    ------------------------------------------------
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-4.2.2/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
...
```
这时候就要安装一下**webrick**了\
`gem install webrick`\
然后再把它添加到bundle：\
`bundle add webrick`\
最后执行：\
`bundle exec jekyll serve`\
成功：
```
PS D:\website\myblog> bundle exec jekyll serve
Configuration file: D:/website/myblog/_config.yml
            Source: D:/website/myblog
       Destination: D:/website/myblog/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.953 seconds.
  Please add the following to your Gemfile to avoid polling for changes:
    gem 'wdm', '>= 0.1.0' if Gem.win_platform?
 Auto-regeneration: enabled for 'D:/website/myblog'
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
  ```
解决这个问题的灵感来着这个[博客](https://velog.io/@minji-o-j/jekyll-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0)
,表示感谢！

*常用命令如下：*
```
gem install xxx
bundle add xxx
bundle update
bundle exec jekyll serve
jekyll serve
```
xxx表名字。

#### 插件
自己对jekyll的插件并不是很了解，但是有一个插件很好用，可以体验一下。

在Gemfile文件中添加`gem "jekyll-admin", group: :jekyll_plugins` 如下：
```
source "https://rubygems.org"
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
gem "jekyll", "~> 4.2.2"
# This is the default theme for new Jekyll sites. You may change this to anything you like.
gem "minima", "~> 2.5"
# If you want to use GitHub Pages, remove the "gem "jekyll"" above and
# uncomment the line below. To upgrade, run `bundle update github-pages`.
# gem "github-pages", group: :jekyll_plugins
# If you have any plugins, put them here!
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds since newer versions of the gem
# do not have a Java counterpart.
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

gem "webrick", "~> 1.7"

gem "jekyll-admin", group: :jekyll_plugins
```
然后执行：\
`bundle install`\
最后执行：\
`bundle exec jekyll serve`\
在`http://127.0.0.1:4000/admin` 即可进入后台。

![](jekyll-admin.png)

#### 写在最后
关于主题有很多，好多漂亮的我改不明白，也不想去花精力去学习改别人的模板。应该去把更多的精力花在写作本身。这也是我为什么用这个主题的原因，这里再次感谢此主题的作者。Github部分不难，我想应该整理一下git的使用，还有Markdown真不错。哈哈哈终，于写完了！