

分支:

1. master生产主分支
2. develop主开发分支
3. feature功能开发分支
4. release测试分支
5. hotfix补丁分支

![gitflow](gitflow.png)

## Master

- **master 分支上存放的是最稳定的正式版本**，并且该分支的代码应该是随时可在生产环境中使用的代码（**Production Ready state**）。当一个版本开发完毕后，产生了一份新的稳定的可供发布的代码时，master 分支上的代码要被更新。同时，每一次更新，都需要在 master 上打上对应的版本号（tag）
- **任何人不允许在 master 上进行代码的直接提交，只接受其他分支的合入**。原则上 master 上的代码必须是合并自经过多轮测试且已经发布一段时间且线上稳定的 release 分支（预发分支）

## Develop

- **develop 分支是主开发分支，其上更新的代码始终反映着下一个发布版本需要交付的新功能**。当 develop 分支到达一个稳定的点并准备好发布时，应该从该点拉取一个 release 分支并附上发布版本号。也有人称 develop 分支为 “integration branch”，因为会基于该分支和持续集成工具做自动化的 Nightly builds
- develop 分支接受其他 Supporting branches 分支的合入，最常见的就是 feature 分支，开发一个新功能时拉取新的 feature 分支，开发完成后再并入 develop 分支。需要注意的是，**合入 develop 的分支必须保证功能完整，不影响 develop 分支的正常运行**。

## Feature

- feature 分支又叫做功能分支，一般命名为 feature/xxx，用于**开发即将发布版本或未来版本的新功能或者探索新功能**。该分支通常存在于开发人员的本地代码库而不要求提交到远程代码库上，除非几个人合作在同一个 feature 分支开发。关于这点，ThoughtWorks 洞见上有一篇文章 “[Gitflow 有害论](https://link.zhihu.com/?target=https%3A//insights.thoughtworks.cn/gitflow-consider-harmful/)” 做了非常有意思的阐述，文章下的评论也异常激烈。也许该文章的名字可能有失偏颇，但文章的本意以及评论传达了一个观点：**feature 分支要求足够细粒度以避免成为 long-lived branch，应当小步小步 merge 而不是一次 merge 大量代码**
- feature 分支只能拉取自 develop 分支，开发完成后要么合并回 develop 分支，要么因为新功能的尝试不如人意而直接丢弃

## Release

- release 分支又叫做预发分支，一般命名为 release/1.2（后面是版本号），**该分支专为测试—发布新的版本而开辟，允许做小量级的 Bug 修复和准备发布版本的元数据信息（版本号、编译时间等）**。通过创建 release 分支，使得 develop 分支得以空闲出来接受下一个版本的新的 feature 分支的合入
- release 分支需要提交到服务器上，交由 QA 进行测试，并由 Dev 修复 Bug。同时根据该分支的特性我们可以部署自动化测试以及生产环境代码的自动化更新和部署
- release 分支只能拉取自 develop 分支，合并回 develop 和 master 分支

## Hotfix

- hotfix 分支又叫热修复分支，一般命名为 hotfix/1.2.1（后面是版本号），**当生产环境的代码（master 上代码）遇到了严重到必须立即修复的缺陷时，就需要从 master 分支上指定的 tag 版本（比如 1.2）拉取 hotfix 分支进行代码的紧急修复，并附上版本号（比如 1.2.1）**。这样做的好处是不会打断正在进行的 develop 分支的开发工作，能够让团队中负责 feature 开发的人与负责 hotfix 的人并行、独立的开展工作
- hotfix 分支只能从 master 上拉取，测试通过后合并回 master 分支和 develop 分支



