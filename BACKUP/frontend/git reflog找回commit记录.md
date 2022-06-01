## git reflog找回commit记录

---

### abbrlink: 21

今天线上出现bug, 在切换到旧版本的时候，由于误操作导致本地代码丢失，找回巨费时，特记录如下; #bug产生原因 首先在master分支上开发，线上出现bug且回到旧版本的tag,这时master分支上有一部分代码修改但未提交。

#### **当前在master上：执行git status 有未提交的代码**

#### **前在master上：执行git tag查看标签信息**

#### **这时未提交代码，执行了git checkout v1.0**

![v2-09de9e74f3d429e31690956ae3b32eec_1440w.jpeg](git%20reflog%E6%89%BE%E5%9B%9Ecommit%E8%AE%B0%E5%BD%95.assets/v2-09de9e74f3d429e31690956ae3b32eec_1440w.jpeg)

#### **当前分支是detached，此时提交git add ./ git commit , 然后又执行了git checkout master，此时detached分支不见了，master上未提交的代码也没有了.....**

#### **代码找回**

#### **执行： git reflog可以看到提交记录**

git co 247e11b

此时，依次执行 git checkout 247e11b git checkout -b diff git checkout master git merge diff

代码成功找回

