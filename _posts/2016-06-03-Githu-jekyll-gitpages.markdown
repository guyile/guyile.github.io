---
layout:     post
title:      "Github + Jekyll + Pygments构建个人博客"
date:       2016-06-03 12:00:00
author:     "Waterstar"
header-img: "img/post-bg-miui6.jpg"
tags:
    - 博客
    
---

#Github + Jekyll + Pygments构建个人博客

文章主题目录：

构建github个人博客库：username.github.io
安装jekyll相关内容
修改configuration file以及建立自己的对应网页
安装相关插件，比如markdown解析包、代码高亮包pygments、mathjax相关修改等，以让博客支持自己的大多数写作需求
本地编译预览，没有问题后上传至github
##1. 构建github个人博客库：username.github.io
不管怎么样去建立这个博客，首先必须在 github 上有这样的一个库，其名称为 username.github.io，username就是每个github用户的用户ID，只有这样名称的库才能被github用于链接到个人博客。
链接的代码就是:

git remote set-url origin git@github.com:*******/********.github.io.git
##2. 安装jekyll相关内容
jekyll 是 Ruby 的一个gem包。安装稍显繁琐，主要顺序如下：

###2.1 下载安装ruby
这里下载的ruby版本为1.9.3，由于大陆墙掉了官方给的Ruby下载网址，所以百度这个版本的下载即可。注意安装路径不能包含中文和空格，就 c://ruby 蛮好。

###2.2 安装 Devkit for Ruby 1.9.3
可到此处下载。并且在cmd.exe中运行如下代码：

cd devkit // 目的是将当前目录转移到devkit解压路径
ruby dk.rb init
ruby dk.rb install
P.S. 安装过程中记得全部勾选 add to the path，将其位置添加到系统变量中，并且在安装完成后，使用如下代码查询安装版本号确认安装成功(显示版本号即为安装成功)：

ruby -v
###2.3 安装 gem 以及 jekyll 包
https://ruby.taobao.org/
到gem下载合适的gem安装包并安装，注意点同上。同时记得这个网站，里面包括了所有的gem功能包，很好狠强大；
安装完gem后也记得查看版本号以及安装完成状态。
然后安装 jekyll,通过

gem install jekyll // 安装jekyll
gem install kramdown // markdown语言解析包
gem install pygments.rb // 代码高亮包
gem install liquid // 这个包我不知道，但是好像有用
// 还有些别的包，看需要来安装啦
其中，高亮包需要python2.6或2.7的环境，因此还需要安装python，所幸在使用某些gem包的时候，可能会出现 Application runtime error的问题，这个时候也需要安装 Node.js 来提供一个使用环境（maybe），也需要安装python，因此就早点安装了更好。

##3. 修改configuration file以及建立自己的对应网页
完成上述操作后，就可以进入博客的成型步骤了。首先需要建立自己的文件档案，也就是博客的硬件来源。有两种方案：

###3.1 自己建立
可以自己通过如下代码建立，会在当前目录下产生一个jekyll默认模板文件夹，里面包含了主要的文件内容，将其整体拷贝到自己的仓库中即可。

jekyll new (name)
###3.2 fork别人
但是更好的方式可能是这种，因为建立一个漂亮的博客是每一个bloger最希望的事，但是可能刚开始入手会有点难，从0开始的话需要安装主题、自己做前段开发是件很考验编程水平和艺术水平的事，不妨可以在刚开始接触的时候先套用别人的模板，等自己对大致使用方法和流程了解后，再在其基础上或自己从0开始构建自己心中最完美的博客。
网上有很多牛人提供了漂亮的主题以及博客模板，因此可以通过github fork，然后将名字改为自己的username.github.io；然后clone到本地（注意路径不能包含中文，因为jekyll在建立运行时对中文不识别导致不成功）。

###3.3 修改相应文件建立自己的blog
库中所包含的主要文件及文件夹有：

文件夹_layouts：用于存放模板的文件夹，
文件夹_posts：用于存放博客文章的文件夹
文件夹css：存放博客所用css的文件夹,比如主题文件以及高亮文件都是放在此处的
.gitignore：可以删掉，后面会将项目添加到git项目，所以这个不需要了
_coinfig.yml：jekyll的配置文件，里面可以定义相当多的配置参数，具体配置参数可以参照其官网
index.html：项目的首页
_includes:用于存放一些固定的HTML代码段，文件为.html格式，可以在模板中通过liquid标签引入，常用来在各个模板中复用如 导航条、标签栏、侧边栏之类的在每个页面上都一样不变的内容，需要注意的是，这个代码段也可以是未被编译的，也就是说也可以使用liquid标签放在这些代码段中
通过修改配置文件_coinfig.yml以及post和layouts就可以实现主要blog的建立和修改。

##4. 安装相关插件，比如markdown解析包、代码高亮包pygments、mathjax相关修改等，以让博客支持自己的大多数写作需求
###4.1 markdown解析包 kramdown
通过gem安装后，需要在配置文件 _coinfig.yml 中添加该项：

markdown: kramdown
kramdown:
  input: GFM
###4.2 代码高亮包 pygments
安装后，首先需要一个 .css 文件，但是我也不清楚怎么在windows下生成这个文件，也就是从网上找到的一个，且其来源为此，并且名字成为 highlight.css，存放目录为 ./assets/css以及./_site/assets/css两个文件夹，然后同样需要在配置文件中做如下修改：

highlight: true
并且需要对样式进行链接，需要修改生成网页的模板或通用头文件，进入本地库目录 ./_includes/header.html,然后添加下句：

<link rel="stylesheet" href="/assets/css/highlight.css">
//其中 /assets/css/ 为该文件所在位置
在使用代码时，需要在符合markdown的语法基础上，在代码主体前后添加两句话，举例如下：

{% highlight c++ linenos %}
{
    cout<<endl;
    cout<<"HAHAHA"<<endl;
    cout<<"I am Hahaha"<<endl;
}
{% endhighlight %}
###4.3 使用mathjax呈现公式
虽然kramdown是支持mathjax语法的，但是为了使网页可以使用mathjax，还需要作如下修改，可参考此处，同样到库 ./_includes/header.html，添加如下语句，有的文章里面给出的添加内容没有这么多，但是多多益善吧，仅仅信之。

##5. 本地编译预览，没有问题后上传至github
###5.1 本地编译预览
使用命令建立并在当地可预览当前的网页状态，网址为：http://localhost:4000/ ，如果有问题则继续修改，没有就可以进行下一步了。

jekyll build 
jekyll serve
###5.2 通过github将本地库的改动提交到网上
所使用的命令为：

git add . // 如果没有新加文件可以不用运行此句
git commit -am '描述内容'
git push (-u origin master) // 括号中内容可加可不加
如果没有提示错误的话，一切顺利，就可以去网站：http://username.github.io 查看属于自己的个人博客啦！~~