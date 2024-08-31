## tvbox_tools
写这个工具的主要原因是网上各种接口重复率和失效率极高。几个多仓接口能有成千上万个线路，实际上不重复、可用的线路只有那么几十个，实在是过于冗余了。所以做了这个整理工具，把接口中所有线路进行去重和格式化，json下载保存为同名txt文件，jar文件保存到jar目录下，最后输出个all.json(包含所有历史下载线路接口)和tvbox.json(本次下载线路接口)文件用于配置app，看起来比较简洁。

## 功能概述
- 支持多仓、单仓、线路接口的私有化(json和对应的jar文件下载到本地，经过格式化后推送到自己的git仓库)
- 支持js动态渲染数据的接口
- 移除失效线路
- 移除名称中的emoj表情
- 根据hash和文件大小去重线路
- 为文件链接自动使用加速 https://mirror.ghproxy.com/

## DEMO

([fish2018/tvbox](https://github.com/fish2018/tvbox))
```bash
docker run --rm -e token=ghp_RSEEieuktNTSxxxxxxxxvSrQJsrBmJ0dAIZw -e username=fish2018 -e url='http://肥猫.com' -e signame='肥猫' 2011820123/tvbox
docker run --rm -e token=ghp_RSEEieuktNTSxxxxxxxxvSrQJsrBmJ0dAIZw -e username=fish2018 -e url='https://www.iyouhun.com/tv/0' 2011820123/tvbox
---
https://mirror.ghproxy.com/https://raw.githubusercontent.com/fish2018/tvbox/master/all.json
https://mirror.ghproxy.com/https://raw.githubusercontent.com/fish2018/tvbox/master/tvbox.json
```

## 运行环境
为了提高解析成功率，V1.9使用requests-html解析js动态渲染的接口，pyppeteer容易因为环境问题异常，docker镜像已经封装相关依赖
- V1.9+ python3.10
- V1.8 python3.6+ 

## 使用方法：

#### 参数选项 
docker run时使用-e选项通过环境变量传参

- [ * ] username or u 指定用户名
- [ * ] token [github.com中设置token](https://github.com/settings/tokens)
- [ * ] url 指定要下载的源，不指定url默认会下载tvbox助手上的线路
- repo 指定你的代码仓库名，默认tvbox
- target 指定你想保存的json文件名，默认tvbox.json
- num 多仓时可以指定下载前num个仓库源
- timeout http请求超时，默认3s
- signame url是单个线路时可以指定线路名(jar同名)，不指定随机生成

#### Docker执行示例:
首先在github.com上创建自己的代码仓库，推荐命名'tvbox'

```bash
docker run --rm  -e username=XXX -e token=XXX -e url='xxx' 2011820123/tvbox
```

国内可以使用代理拉取镜像
```bash
docker run --rm  -e username=XXX -e token=XXX -e url='xxx' dockerproxy.com/2011820123/tvbox:latest
```

## 更新说明
- V1.9版本 移除多线程下载接口；已下载接口不重复下载；支持js动态渲染数据的接口；增加根据文件大小去重线路；单线路下载不指定signame(单线路名)时会生成一个"{随机字符串}.txt"；兼容主分支main/master
- V1.8版本 移除agit.ai支持；all.json线路排序；
- V1.7版本 优化git clone速度，仓库重置提交记录
- V1.6版本 不规范json兼容优化，http请求timeout默认3s，优化移除emoji表情
- V1.5版本 bug修复，github.com支持优化
- V1.4版本 bug修复，jar下载失败，不会创建0字节jar文件，保留原jar链接
- V1.3版本 支持github.com
- V1.2版本 支持jar本地化
- V1.1版本 bug修复，仅支持agit.ai，不支持jar本地化
- V1.0版本 支持单线路、单仓、多仓下载，输出：{target}(默认tvbox.json)，和url填写的源内容一致；all.json是仓库中所有下载的历史线路总和，并且去重了内容相同的线路
  
