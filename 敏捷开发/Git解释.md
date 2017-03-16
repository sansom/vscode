# Git解释

## Tag问题

每一个在master发布的RELEASE版本应该提交一个Tag。

命名方式：版本号+时间

## 回滚问题

![](http://of0qa2hzs.bkt.clouddn.com/git2.png)

如图所示：

1. 完成1.0.1-RELEASE，发布到线上，新增Tag：1.0.1-RELEASE。

    master版本：1.0.1-RELEASE，线上版本：1.0.1-RELEASE。

2. 完成1.0.2-RELEASE，发布到线上，新增Tag：1.0.2-RELEASE。

    master版本：1.0.2-RELEASE，线上版本：1.0.2-RELEASE。

3. 发现重大bug，必须要马上修复。马上就是立刻，所以必须要直接回滚。操作如下：

    a.使用1.0.2-RELEASE建立hot分支。

    b.通过Tag将1.0.2-RELEASE回滚到1.0.1-RELEASE，发布到线上。

   master版本：1.0.1-RELEASE，线上版本：1.0.1-RELEASE

4. 在hot分支上修复bug，发布到线上，新增Tag：1.0.3-RELEASE。同时merge到正在开发的dev1.0.2（正在开发的功能）。

   master版本：1.0.3-RELEASE，线上版本：1.0.3-RELEASE

重大bug：

    什么是重大bug需要自己定义。但是重大bug的解决方案是立刻回滚，所以说如果客户B已经接入了1.0.2的功能，
    这个时候你就应该考虑这个bug是否需要立刻回滚。
    非重大bug指不需要回滚的bug，直接建立hot分支修改，没有回滚步骤。
