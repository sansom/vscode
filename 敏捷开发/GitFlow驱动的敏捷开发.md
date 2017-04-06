# GitFlow驱动的敏捷开发

## 说明

虽然我们实行敏捷开发很久了，但是确实遇到了很多问题，实践反映我们并不够敏捷。

Git的使用已经互相分享和讨论了很多次，我们使用git来做版本管理等都是人工和手动的（当然也完成了git分支模型的内容）。

人工和手动带来的问题就是非常的不标准，流程混乱以及容易出错。

当然频繁的需求与发布是主要原因，但是这是任何一个公司都无法避免的。

我们应该改变自己来达成目标。

所以现在希望使用git flow来替代我们的手动做法。

## GitFlow基础

- 分支模型

    Git开发的分支模型已经讲过很多次了，但是实践中仍然有很多误区以及很多没有实现的地方。

    分支模型文章：[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

    ![](http://of0qa2hzs.bkt.clouddn.com/1491374868%281%29.jpg)
- git flow 初始化

    - 命令行

        `git flow init` ：初始化一个git本地仓库。

        接着回答几个关于分支的问题。使用默认值即可，直接按回车键。

        ```
        $ git flow init

        Which branch should be used for bringing forth production releases?
            - master
        Branch name for production releases: [master]
        Branch name for "next release" development: [develop]

        How to name your supporting branch prefixes?
        Feature branches? [feature/]
        Bugfix branches? [bugfix/]
        Release branches? [release/]
        Hotfix branches? [hotfix/]
        Support branches? [support/]
        Version tag prefix? []
        Hooks and filters directory? [E:/NEWWORK/h5-inst-system/.git/hooks]
        ```
    - IDEA 插件

        `Git Flow Integration` ：安装插件即可

        ![](http://of0qa2hzs.bkt.clouddn.com/idea-git-flow.png)

- 分支与常用命令

    - Master

        `master`分支只有一个。 

        `master`分支上的代码总是稳定的，随时可以发布出去。

        平时一般不在`master`分支上操作，当`release`分支和`hotfix`分支合并代码到`master`分支上时，`master`上代码才更新。 

        当仓库创建时，`master`分支会自己创建。

    - Develop

        `develop`分支只有一个。 

        新特性的开发是基于`develop`分支的，但不直接在`develop`分支上开发，特性的开发是在`feature`分支上进行。 

        当`develop`分支上的特性足够多以至于可以进行新版本的发布时，可以创建`release`分支的。

    - Feature

        可以同时存在多个`feature`分支，新特性的开发正是在此分支上面。

        可以对每个新特性创建一个新的`feature`分支，当该特性开发完毕，将此`feature`分支合并到`develop`分支。

        创建一个新的`feature`分支，可以使用以下命令：

        > git flow feature start test
        
        执行以下命令后，`feature/test`分支会被创建。 

        当特性开发完毕，需要将此分支合并到`develop`分支，可以使用以下命令实现：

        > git flow feature finish test

        上面的命令会将`feature/test`分支的内容`merge`到`develop`分支，并将`feature/test`分支删除。

        `feature`分支只是存在于本地仓库，如果需要多个人共同开发此特性，也可以将`feature`分支推送到过程仓库。

        > git flow feature publish test

        `feature` 分支的生命周期持续到特性的开发完毕，当完成特性的开发，你可以使用git的分支管理命令将此`feature`分支删除。

    - Release

        当完成了特性的开发，并且将`feature`分支上的内容`merge`到`develop`分支上，这时可以开始着手准备新版本的发布，`release`分支正是作为发布而开设的分支。 

        `release`分支基于`develop`分支，在同一时间只有一个`release`分支，其生命周期较短，只是为了发布而使用。
        
        这意味着，在`release`分支上，只是进行较少代码修改，比如bug的修复，原有功能的完善等。
        
        不允许在`release`分支增加大的功能，因为这样会导致`release`分支的不稳定，不利于发布的进行。 

        当`release`分支（例如，v.1.0）被创建出来后，`develop`分支可能正准备另一版本（例如，v.2.0），
        
        因此，当`release`分支`merge`回`develop`分支时，可能会出现冲突，需要手工解决冲突才能继续`merge`。

        通过以下命令来创建`release`分支：
        > git flow release start v.1.0

        执行过完上面的命令，`release`分支`release/v.1.0`会被创建出来 ，并且切换到该分支。

        当完成`release`分支功能的完善或者bug的修复后，执行以下命令来完成`release`分支：

        > git flow release finish v.1.0
    - Hotfix

        当发现`master`分支出现一个需要紧急修复的bug，可以使用`hotfix`分支。`hotfix`分支基于`master`分支，用来修复bug，
        
        当完成bug的修复工作后，需要将其`merge`回`master`分支。 

        同一时间只有一个`hotfix`分支，其生命周期较短。

        可以使用以下命令来创建`hotfix`分支：

        > git flow hotfix start v.1.0

        使用以下命令来结束`hotfix`分支的生命周期：

        > git flow hotfix finish v.1.0

        这句命令会将`hotfix`分支`merge`到`master`分支和`release`分支，并删除该`hotfix`分支。 
        注意，如果bug修复时，正存在着`release`分支，那么`hotfix`分支会`merge`到`release`分支，而不是`develop`分支。


## 更进一步

`git flow` + `pull request` + `code review`

我们现在做到的流程有：人工的git分支管理+比较随意的分支合并+滞后的code review

或许我们可以尝试一下，一步到位。

使用`git flow`来管理项目开发，开发完成之后`pull request`来做分支合并管理，收到`request`之后对合并的代码进行`code review`保证质量


## 流程规范

- Feature分支的周期及流程规范

    当客户产生新需求时，`项目负责人`应该拆分需求，不同的`开发`负责不同的任务，建立不同的`Feature`。

    Feature分支都是基于Dev分支的，当`Feature`分支开发完成之后，需要`测试介入`测试。

    说明：对于其他公司来说，可能一个`Feature`就有好几个在一起开发。我们公司项目比较多，人员比较少。

    可能一个`Featur`e就两个人开发，`一个后台+一个前端`，但是我觉得任何可以由后台来负责整个`Futura`，把我自己`Futura`的进度。

    我们前面遇到的问题就是因为前后没有沟通，现在何不确认一个主次，让后端开发的人员来主动一点，跟进进度，负责`Futura`。

- Feature合并到Dev

    测试完成之后的`Feature`分支发`pull request`到`Dev`分支，`项目负责人`做`code review`并验收。

- Dev工作流

    当所有需求的新特性开发完成之后，测试做`回归测试`+`验收测试`，负责人发布版本到`RELEASE`。

- RELEASE工作流

    预上线版本。

## 挑战

- 开发人员

    对于开发来说，变化并不大。由原来的自建`Futura分支`改成了`git flow`的分支。可能原来的`Futura分支`会一直保存为`邓浩梁分支`。

    但是现在每一个`Futura`都只为功能服务，当某一个功能开发完成之后本`Futura`就会消失，等待下一个功能建立新的`Futura`。

- 测试

    测试的任务将会稍微变多一点。第一要测试`所有的Futura`，第二当所有`Futura`合并到`De`v的时候要`回归测试原来的Futura`和`综合测试最终的Dev`。

- 项目负责人

    项目负责人将承担更多的责任，不仅有原来的`任务分发`，还有`自身要参与的项目内容`，最后还要`负责每一个Futura的质量验收`。


## 可能遇到的问题

- 每个`feature`都是一个独立的需求（或功能），一定要全部测试通过后才能合并到`dev`，否则后期就是大家都去`dev`上开发了。

- 在feature开发过程中发现develop上的代码有bug

    `dev`分支应该是共有的分支，每一个`Futura`都是基于`dev`的。所以每一天开发都应该在早晨`pull一下dev`。
    
    如果是周期短的项目，遇到bug情况，`dev`的开发者应该通知所有`Futura`的开发者pull代码。

- hotfix 问题,本问题已经讨论过，不在赘述。



