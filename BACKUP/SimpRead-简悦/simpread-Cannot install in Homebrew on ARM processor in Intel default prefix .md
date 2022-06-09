# simpread-Cannot install in Homebrew on ARM processor in Intel default prefix 

# simpread-Cannot install in Homebrew on ARM processor in Intel default prefix

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [www.jianshu.com](https://www.jianshu.com/p/8d0b42470679)
>

处理 homebrew 无法在 M1 芯片的 Mac 上安装软件的问题。这是在新的 M1 芯片的 MacBookpro 中，在使用 homebrew 安装 CocoaPods 遇到的问题。

为了不翻墙也能很好的使用，所以采用是国内镜像的安装方式。

### 首先，使用国内镜像安装 homebrew

```other
/bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
```

执行之后的结果是

```other
开始执行Brew自动安装程序
...
请选择一个下载镜像，例如中科大，输入1回车。
源有时候不稳定，如果git克隆报错重新运行脚本选择源。cask非必须，有部分人需要。
1、中科大下载源 2、清华大学下载源 3、北京外国语大学下载源 4、腾讯下载源（不显示下载进度） 5、阿里巴巴下载源(缺少cask源)
```

我一开始选择的是第 2 个，离的太远，网络还是跟不上，最后下载使用。重新执行上述命令选择镜像 1，安装成功。

安装成功之后的提示：

```other
Already up-to-date.

        上一句如果提示Already up-to-date表示成功
            Brew自动安装程序运行完成
              国内地址已经配置完成

                初步介绍几个brew命令

        本地软件库列表：brew ls
        查找软件：brew search google（其中google替换为要查找的软件关键字）
        查看brew版本：brew -v  更新brew版本：brew update

现在可以输入命令open ~/.zshrc -e 或者 open ~/.bash_profile -e 整理一下重复的语句(运行 echo $SHELL 可以查看应该打开那一个文件修改)
```

使用 `open ~/.zshrc -e`可以看到 brew 相关的环境变量配置。因为脚本默认安装将 homebrew 安装在了`/usr/local`目录下，所以，环境变量所使用的 PATH 默认执行此目录。

当我们使用 brew insatll something 的时候，在 M1 芯片的 Mac 中回报如下错误

```other
Error: Cannot install in Homebrew on ARM processor in Intel default prefix (/usr/local)!
Please create a new installation in /opt/homebrew using one of the
"Alternative Installs" from:
  https://docs.brew.sh/Installation
You can migrate your previously installed formula list with:
  brew bundle dump
```

也就是说我们不能在 ARM 处理器中使用 Intel 的默认前缀 / usr/local。请在 / opt/Homebrew 中安装。

如果我们一开始就把 homebrew 安装到 /opt/homebrew 而不是 /usr/local 就好了。但是安装脚本似乎没有提供修改安装路径的方式。所以，我想可以将安装目录从 / usr/local 迁移到 / opt/homebrew 即可。事实上这样做确实可以。

我的电脑是新电脑，/usr/local 目录下还很纯洁，基本上都是之前安装 homebrew 产生的文件。

1. 将 / usr/local 目录下的所有文件移动到 /opt/homebrew 目录下

```other
sudo mkdir /opt/homebrew
sudo mv /usr/local/* /opt/homebrew
```

2. 修正 / bin 目录下的软连接

```other
ln -s /opt/homebrew/Homebrew/bin/brew /opt/homebrew/bin/brew
```

3. 修改环境变量

```other
open ~/.zshrc -e
```

编辑此文件，将内容修改如下

```other
# HomeBrew
export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles
export PATH="/opt/homebrew/bin:$PATH"
export PATH="/opt/homebrew/sbin:$PATH"
# HomeBrew END
```

主要是修改 path 路径。

上面 3 步，所有工作已经完工。

此时执行 brew -v 会提示找不到 brew 命令。这是因为在当前的 shell 中 brew 还是原来的环境变量，不知道改变后的 brew 在哪里。所以关闭 shell 重新打开，或者执行`source ~/.zshrc`重新载入即可。

## 然后我们就可以使用 homebrew 安装需要的东西了。

在使用 homebrew 安装之前，要切换下源。因为默认的源实在太慢了，你可以 ping 一下试试。

```other
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
> /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/universal-darwin20/rbconfig.rb:229: warning: Insecure world writable dir /opt/homebrew/sbin in PATH, mode 040777
> https://gems.ruby-china.com/ added to sources
> https://rubygems.org/ removed from sources
```

切换源之后，我们安装下 cocopods

```other
sudo gem install -n /opt/homebrew/bin cocoapods
```

很顺利就完成了。
