#git-flow 使用


##安装 git-flow
    
    brew install git-flow

*要了解安装 git-flow 细节，请阅读下面这个文档 [official documentation](https://github.com/petervanderdoes/gitflow/wiki#installing-git-flow)。*

##在项目中设置 git-flow
    
    ➜  GitFlow git:(master) git flow init

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
    Hooks and filters directory? [/Users/tonytong/Desktop/GitFlow/.git/hooks


##功能开发
对于一个开发人员来说，最平常的工作可能就是功能的开发。这就是为什么 git-flow 定义了很多对于功能开发的工作流程，从而来帮助你有组织地完成它。

**Feature 开始开发一个新功能**
    

    ➜  GitFlow git:(develop) git flow feature start test-gitflow
    Switched to a new branch 'feature/test-gitflow'
    
    Summary of actions:
    - A new branch 'feature/test-gitflow' was created, based on 'develop'
    - You are now on branch 'feature/test-gitflow'
    
    Now, start committing on your feature. When done, use:
    
         git flow feature finish test-gitflow
         
    
    上面的 git flow feature start test-gitflow 会基于develop创建一个新的分支test-gitflow 并自动切换到该分支


**Feature 完成一个新功能**
    
    ➜  GitFlow git:(feature/test-gitflow) git flow feature finish test-gitflow
    Switched to branch 'develop'
    Updating 862396e..e367e7c
    Fast-forward
     GitFlow/ViewController.swift | 2 ++
     1 file changed, 2 insertions(+)
    Deleted branch feature/test-gitflow (was e367e7c).
    
    Summary of actions:
    - The feature branch 'feature/test-gitflow' was merged into 'develop'
    - Feature branch 'feature/test-gitflow' has been locally deleted
    - You are now on branch 'develop'

执行feature finish之后会把我们的工作整合到主 “develop” 分支中去 
而且还会删除这个当下已经完成的功能分支 并且自动切换到 “develop” 分支 

**创建Release**

    ➜  GitFlow git:(develop) git flow release start 1.0.0
    Switched to a new branch 'release/1.0.0'
    
    Summary of actions:
    - A new branch 'release/1.0.0' was created, based on 'develop'
    - You are now on branch 'release/1.0.0'
    
    Follow-up actions:
    - Bump the version number now!
    - Start committing last-minute fixes in preparing your release
    - When done, run:
    
         git flow release finish '1.0.0'
         
    
请注意，release 分支是使用版本号命名的。这个命名方案还有一个很好的附带功能，那就是当我们完成了release 后，git-flow 会适当地_自动_去标记那些 release 提交。

有了一个 release 分支，再完成针对 release 版本号的最后准备工作（如果项目里的某些文件需要记录版本号），并且进行最后的编辑。


         
**完成Release**

    ➜  GitFlow git:(release/1.0.0) git flow release finish -m "*release) finish"
    Switched to branch 'master'
    Merge made by the 'recursive' strategy.
     GitFlow/ViewController.swift | 1 +
     1 file changed, 1 insertion(+)
    Already on 'master'
    Switched to branch 'develop'
    Already up to date!
    Merge made by the 'recursive' strategy.
    Deleted branch release/1.0.0 (was 93ae994).
    
    Summary of actions:
    - Release branch 'release/1.0.0' has been merged into 'master'
    - The release was tagged '1.0.0'
    - Release tag '1.0.0' has been back-merged into 'develop'
    - Release branch 'release/1.0.0' has been locally deleted
    - You are now on branch 'develop'


    这个命令会完成如下一系列的操作：

    1 首先，git-flow 会拉取远程仓库，以确保目前是最新的版本。
    2 然后，release 的内容会被合并到 “master” 和 “develop” 两个分支中去，这样不仅产品代码为最新的版本，而且新的功能分支也将基于最新代码。
    3 为便于识别和做历史参考，release 提交会被标记上这个 release 的名字（在我们的例子里是 “1.0.0”）。
    4 清理操作，版本分支会被删除，并且回到 “develop”。
    
    从 Git 的角度来看，release 版本现在已经完成。依据你的设置，
    对 “master” 的提交可能已经触发了你所定义的部署流程，或者你可以通过手动部署，
    来让你的软件产品进入你的用户手中。


**创建Hotfix**

    ➜  GitFlow git:(develop) git flow hotfix start crash-login
    Switched to a new branch 'hotfix/crash-login'
    
    Summary of actions:
    - A new branch 'hotfix/crash-login' was created, based on 'master'
    - You are now on branch 'hotfix/crash-login'
    
    Follow-up actions:
    - Start committing your hot fixes
    - Bump the version number now!
    - When done, run:
    
         git flow hotfix finish 'crash-login'

**完成Hotfix**

    ➜  GitFlow git:(master) git flow hotfix finish crash-login -m "*bugfix) finish"
    Switched to branch 'develop'
    Merge made by the 'recursive' strategy.
     GitFlow/ViewController.swift | 1 +
     1 file changed, 1 insertion(+)
    Deleted branch hotfix/crash-login (was 1330d0b).
    
    Summary of actions:
    - Hotfix branch 'hotfix/crash-login' has been merged into 'master'
    - The hotfix was tagged 'crash-login'
    - Hotfix tag 'crash-login' has been back-merged into 'develop'
    - Hotfix branch 'hotfix/crash-login' has been locally deleted
    - You are now on branch 'develop'



##提交commit规范

    feat: 新增feature
    fix: 修复bug
    docs: 仅仅修改了文档，比如README, CHANGELOG, CONTRIBUTE等等
    style: 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑
    refactor: 代码重构，没有加新功能或者修复bug
    perf: 优化相关，比如提升性能、体验
    test: 测试用例，包括单元测试、集成测试等
    chore: 改变构建流程、或者增加依赖库、工具等
    revert: 回滚到上一个版本
    
    eg:
        git commit -m "feat： balabalabalaxxxxxxxxxx"
        git commit -am "fix： balabalabalaxxxxxxxxxx"


##问题

**feature是否应该细化到一个个小功能？**
**feature与feature之间有耦合怎么处理？**


##参考
[GitFlow with SourceTree](https://www.jianshu.com/p/8a3988057d0f)
[git-flow 的工作流程](https://www.git-tower.com/learn/git/ebook/cn/command-line/advanced-topics/git-flow)

