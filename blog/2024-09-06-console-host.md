---
slug: console-host
title: 控制台配代理
tags: [tricks]
---

> 参考：[https://weilining.github.io/294.html](https://weilining.github.io/294.html)
> 亲测好用，记录一下

## 环境

macOS，使用 ClashX 1.117.1，ClashX 正常代理，使用自带终端。

## 场景

克隆 git 仓库、python 下依赖、使用 homebrew、npm 等

<!-- truncate -->

## 配置

打开终端，输入以下代码
`7890` 是 clash 对 http 代理对应的端口的默认值，根据实际情况配置，在 Clash 的设置界面中查找代理端口信息。

```bash
cat > ~/.bash_profile << EOF
function proxy_on() {
    export http_proxy=http://127.0.0.1:7890
    export https_proxy=\$http_proxy
    echo -e "终端代理已开启。"
}

function proxy_off(){
    unset http_proxy https_proxy
    echo -e "终端代理已关闭。"
}
EOF



```

开启代理

```bash
source ~/.bash_profile
proxy_on
```

关闭代理

```bash
source ~/.bash_profile
proxy_off
```

## 上上谷歌检测一下

```bash
curl -I http://www.google.com
```
