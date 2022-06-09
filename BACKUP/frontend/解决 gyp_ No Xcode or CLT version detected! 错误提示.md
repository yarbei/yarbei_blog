# 解决 gyp_ No Xcode or CLT version detected! 错误提示

# 解决 gyp_ No Xcode or CLT version detected! 错误提示

---

更新Mac系统后，在用npm安装依赖包的时候总会报这个错误：gyp: No Xcode or CLT version detected!

查找了网上的解决办法，基本上都是清一色的说执行这个命令即可：sudo xcode-select --install

可执行后安装npm包还是会报上面的错误提示，虽然没什么实质的影响，但看着这错误提示不舒服，便继续寻找解决的办法。

功夫不负有心人，在国外的一个网站上找到了解法：

- Step1: 输入xcode-select --print-path查看 command-line tools 的安装路径，不出意外显示的结果应该是/Library/Developer/CommandLineTools
- Step2: 输入sudo rm -r -f /Library/Developer/CommandLineTools把 command-line tools 从系统移除掉
- Step3: 最后输入xcode-select --install重新安装
