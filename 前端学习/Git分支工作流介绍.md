# Git分支工作流介绍

原文链接：https://juejin.cn/post/7202952940196708410

​		本文主要讲解Git 的工作流程，不涉及到基本的使用，如果想学习Git的基本操作可以访问http://www.canlee.top/?#/article/8。

​		Git是目前最流行的的代码管理工具，开发团队一般为了规范开发，保持良好的代码提交记录以及维护Git分支结构清晰，方便后序的维护等，会制定一套团队内部的不叫规范的Git工作流。本文主要讲解最为常见和流行的三种Git分支工作流。

## You need  to know

> **生产环境（Production Environment）**：
>
> - 生产环境是最终用户使用的实际运行环境，其中部署着已经通过测试并准备好供用户使用的软件版本。
> - 在生产环境中，应用程序承担着真实的工作负载，因此稳定性、性能和安全性是至关重要的。
> - 任何对生产环境的更改都需要经过严格的测试和审批，并且应该尽量减少对用户的影响。
>
> **开发环境（Development Environment）**：
>
> - 开发环境是开发人员用于编写、测试和调试软件的环境。
> - 在开发环境中，开发人员可以快速迭代、调试和修改代码，以实现新功能或修复问题。
> - 开发环境通常与实际的生产环境相分离，因此不需要考虑实际用户的体验或数据的安全性。
>
> **Pull Request (PR)** 
>
> Pull Request (PR) 是一种在版本控制系统中常见的功能，例如 Git 中的 GitHub、GitLab 等平台。它是一种协作机制，用于请求其他开发人员审查和合并代码更改。
>
> 基本工作流程如下：
>
> 1. **创建 Pull Request**：开发人员在自己的分支上完成工作后，创建一个 Pull Request，请求将自己的更改合并到目标分支中。
> 2. **填写描述**：在创建 Pull Request 时，开发人员提供有关更改的描述，包括目的和解决的问题。
> 3. **审查和讨论**：其他团队成员对代码进行审查，并提供反馈和建议。开发人员在讨论中回应并进行修改。
> 4. **持续集成和测试**：通常会触发持续集成流程，确保代码通过所有测试。
> 5. **合并 Pull Request**：一旦代码经过审查并通过测试，可以合并到目标分支中。


## GitFlow

---

GitFlow 是最早诞生，并且得到广泛应用的一种工作流程。GitFlow 通常包含五种类型的分支，分别为master、develop、feature、release、hotfix。

![spaces_E4jf29CYQLykrN31w7tP_uploads_lIxPYs5D1wiozRZNMlyZ_【GitFlow】gitflow.webp](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/49e51a56668b4628adcde4a31eb886e0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

- **master**：主干分支，也是正式发布版本的分支，每次发布版本都需要打上相应的 tag(版本号)，其包含可以部署到生产环境中的代码，通常情况下只允许其他分支将代码合入，不允许直接向 master 分支直接提交代码 (master 对应着生产环境)。
- **develop**：开发分支，在开发初期，由团队负责人从 master 签出，用来集成测试最新合入的开发成果，包含要发布到下一个 Release 的代码（对应开发环境）。
- **feature**：特性分支，通常从 Develop 分支签出，每个新特性的开发对应一个特性分支，用于开发人员提交代码并进行自测。自测完成后，会将 Feature 分支的代码合并至 Develop 分支，进入下一个Release 环节。
- **release**：预发布分支，发布新版本时，基于 Develop 分支创建，发布完成后，需要将此分支合并到 Master 和 Develop 分支（对应集成测试环境）。
- **hotfix**：热修复分支，生产环境发现新 Bug 时创建的临时分支，问题验证通过后，合并到 Master 和 Develop 分支

​		`	develop`分支包含项目完整的历史记录，而`master`分支将包含简化版本。团队成员在克隆中央存储库后，应基于`develop`创建跟踪分支，进行新功能开发。

### GitFlow的优点和缺点？

优点：

1. **清晰的分支模型**：GitFlow 提供了一种清晰的分支模型，将开发、发布和维护分开，使团队更容易理解和遵循。
2. **版本控制**：GitFlow 通过在主分支上维护稳定的版本，使得版本控制更加清晰和可管理。
3. **支持并行开发**：通过使用不同的分支来处理不同的功能和问题，GitFlow 支持团队并行开发多个功能。
4. **容易与持续集成集成**：GitFlow 的结构使得与持续集成工具（如 **Jenkins***、Travis CI 等）更容易集成，从而实现自动化测试和部署。
5. **清晰的责任分配**：GitFlow 将开发、测试、发布等角色分配到不同的分支上，使得责任更加清晰。

缺点：

1. **管理相对复杂，需要同时维护两个长期分支**：GitFlow 要求同时维护主分支（**master**）和开发分支（**develop**），这增加了管理的复杂性。开发者需要确保两个分支之间的同步和正确的合并，这可能导致错误和混乱。
2. **不适合持续发布**：GitFlow 更适合基于版本发布的开发流程，而不是持续发布。这意味着目标是在一段时间后产出一个新版本，而不是根据功能完成情况随时发布。对于需要频繁发布、快速反馈的团队或项目，GitFlow 可能会显得不够灵活，因为它强调版本间的稳定性和整合

### 开发分支（Develop）

Develop 分支由团队负责人从 Master 创建。首次创建时，由团队负责人构建项目结构，推送至远端。通常在该分支上进行新特性的合并、签出予发布分支、合并`hotfix`分支等操作。

### 特性分支（Feature）

新特性分支基于 Develop 分支创建，每个新功能都应该驻留在其自己的分支中，我们可以将其推送到中央存储库进行协作或者备份。

在新功能开发完成后，需要将其合并到 Develop 分支中，同时删除本地与远端的`feature`分支。

### 预发布分支（Release）

一旦 Develop 分支获得了足够的发布功能，或者临近预定的发布日期，就需要基于 Develop 分支创建Release 分支。

创建此分支意味着将开始下一个发行周期，此刻 Release 分支不能添加任何新功能（除了错误修复、文档完善等），一旦准备发布，Release 分支将合并到 Master 分支并用版本号标记。另外，还要将其合并回 Develop 分支。待发版成功后，删除 Release 分支。

- 专门的分支进行版本发布可以让我们在新版本发布的同时，其他团队成员可以继续为下个版本开发新功能，而不影响此次发布。

### 补丁分支（hotfix）

维护 hotfix 分支用于快速修补生产版本出现的 bug。hotfix 分支是唯一一个从 Master 分支上创建的分支。

待修复程序完成后，应该将其合并到 Master 和 Develop 或者当前 Release 分支中，随后删除 hotfix 分支，并应使用更新的版本号标记 Master 分支。

- 专门的错误修复分支，可以保障团队可以在不中断其余工作流程的情况下，对线上的版本进行错误修复。

## Github Flow

---

GitHub flow 是 Git Flow 的简化版，它是 [github.com](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2F) 使用的工作流程。它只有一个长期分支(master)，相对于 GitFlow 用起来相对比较简单。GitHubFlow 假设每次合并一个特性分支时都可以部署到生产环境，虽然这在某些情况下是不可能的。（在我们一般个人开发常用到）

![img](https://cloud-pic.wpsgo.com/Wmt2MmZBdUtIWG9qb2dNNGovbnBlRnVpa3k5dmV4RWlCdURJbm5uWDJNSDJOd2I1UTVKVTJwOEsxQTBjdXUxdnBQdFZyNXEwMjBLbkZyZUZUTFBjS1dKbHB3MG9zVy9pY1I5NTJEdjVPK3Ezb3c5VzRBc0FicS9YMUtRUmRqOXRDaFJkRUlsTUZKcmJZMWQ0Y0laMnM2TUxkVkEzejR2enpsTkhxemVlTDAxcFdKdEdhRURISzRHWXZHKzlibDdjSUFLNTYzSGM5dVpUS2p0a1lWdWxUZVRCZGZiVUNITTdRb0p4Z01QRXFNN3dWVm01ZkM2SjVHSE1haW5vQTd3L2VHZmRqNjJCcWwyb1ZiSUNtdWZGbFIzb2V5eFU3UGJncnBSUk5MQ1JVRVBMamJJNTR2LzlNaURlcm9IYg==/attach/object/5fc087f7377975a264162fb0ff906dc88d490b3b?)

它只有一个长期分支master：

- 第一步：根据需求，从 master 拉出新分支，不区分功能分支或补丁分支。
- 第二步：新分支开发完成后，或者需要讨论的时候，就像 master 分支发起一个pull request(简称PR)。
- 第三步：Pull Request既是一个通知，让别人注意到你的请求，又是一种对话机制，大家一起评审和讨论你的代码，对话过程中，你还可以不断提交代码。
- 第四步：你的Pull Request被接受，合并进master，重新部署后，原来你拉出来的那个分支就被删除。

#### GitHub Flow的优点和缺点？

优点：非常适合持续发布的产品，使用流程简单，仅有一个主干分支。Master 分支的最新代码，默认就是线上代码。

缺点：master 既包含生产环境，又包含开发环境，比较混乱。有些时候，代码合并进入 Master 分支，并不代表就能立刻发布，你不能控制发布的时间。例如，iOS 应用程序通过App Store验证后才发布。这是，如果后面还有新的代码合并到 Master，导致与刚刚发布的版本不一致。在这些情况下，需要额外创建一个 production 分支跟踪线上版本。如果想要查看生产环境代码，可切换至生产分支。

## Gitlab Flow

Gitlab Flow 是 Git Flow 与 Github Flow 的结合。它吸取了两者的优点，既有适应不同开发环境的弹性，又有单一分支的简单和便利，是 [gitlab.com](https://link.juejin.cn/?target=https%3A%2F%2Fabout.gitlab.com%2F) 推荐的做法。

### 上游优先

Gitlab Flow 的最大原则叫做“上游优先”，指存在一个主分支，他是所有其他分支的上游，只有上游分支采纳的代码变化，才能应用到其他分支。

### 持续发布

对于持续发布的项目，建议在 Master 之外，创建不同环境的分支，比如开发环境是 Master，预发布分支是`pre-production`，生产环境是`production`。

![img](https://cloud-pic.wpsgo.com/Wmt2MmZBdUtIWG9qb2dNNGovbnBlRnVpa3k5dmV4RWlCdURJbm5uWDJNSDJOd2I1UTVKVTJwOEsxQTBjdXUxdnBQdFZyNXEwMjBLbkZyZUZUTFBjS1dKbHB3MG9zVy9pY1I5NTJEdjVPK3Ezb3c5VzRBc0FicS9YMUtRUmRqOXRDaFJkRUlsTUZKcmJZMWQ0Y0laMnM2TUxkVkEzejR2enpsTkhxemVlTDAxcFdKdEdhRURISzRHWXZHKzlibDdjSUFLNTYzSGM5dVpUS2p0a1lWdWxUZVRCZGZiVUNITTdRb0p4Z01QRXFNN3dWVm01ZkM2SjVHSE1haW5vQTd3L2VHZmRqNjJCcWwyb1ZiSUNtdWZGbFIzb2V5eFU3UGJncnBSUk5MQ1JVRVBMamJJNTR2LzlNaURlcm9IYg==/attach/object/29a0524d6821d7ea4e43758ac6e84df9b485ab82?)

开发分支是语发布分支的上游，预发布分支是生产分支的上游。代码变化，必须由上游向下游发展。

### 版本发布

对于版本发布的项目，建议的做法是每一个稳定的版本，都要从 Master 分支上拉出一个分支，比如`1.0.3-stable`等等。

![img](https://cloud-pic.wpsgo.com/Wmt2MmZBdUtIWG9qb2dNNGovbnBlRnVpa3k5dmV4RWlCdURJbm5uWDJNSDJOd2I1UTVKVTJwOEsxQTBjdXUxdnBQdFZyNXEwMjBLbkZyZUZUTFBjS1dKbHB3MG9zVy9pY1I5NTJEdjVPK3Ezb3c5VzRBc0FicS9YMUtRUmRqOXRDaFJkRUlsTUZKcmJZMWQ0Y0laMnM2TUxkVkEzejR2enpsTkhxemVlTDAxcFdKdEdhRURISzRHWXZHKzlibDdjSUFLNTYzSGM5dVpUS2p0a1lWdWxUZVRCZGZiVUNITTdRb0p4Z01QRXFNN3dWVm01ZkM2SjVHSE1haW5vQTd3L2VHZmRqNjJCcWwyb1ZiSUNtdWZGbFIzb2V5eFU3UGJncnBSUk5MQ1JVRVBMamJJNTR2LzlNaURlcm9IYg==/attach/object/1ec42780b39956e8a3c13d22591ee6e858a839dc?)

发布版本后，只有向该分支添加严重的错误修复，首先将错误修复合并到 Master 分支，然后将其合并到 stable 分支，并且此时要更新小版本号码。

要使用 GitLab Flow 开发项目，可以按照以下步骤进行：

1. **设置 GitLab 项目**：
   - 在 GitLab 上创建一个新项目，或者使用现有的项目。
   - 确保项目已经配置好了 GitLab CI/CD，以便进行持续集成和持续交付。
2. **创建主分支**：
   - 在项目中创建一个主分支，通常使用 `master` 作为主分支的名称。
3. **创建 Feature 分支**：
   - 每当需要开发新功能时，从主分支拉取一个新的 Feature 分支。
   - 分支命名可以根据功能进行描述，例如 `feature/add-login-page`。
4. **开发新功能**：
   - 在 Feature 分支上进行新功能的开发。在这个阶段，开发人员可以自由地编写、测试和调试代码。
5. **提交并推送代码**：
   - 当新功能开发完成并通过了本地测试后，将代码提交到 Feature 分支，并将分支推送到 GitLab 服务器。
6. **发起 Pull Request**：
   - 在 GitLab 上创建一个 Pull Request，将 Feature 分支的更改合并到主分支中。
   - 在 Pull Request 中，可以添加详细的描述、相关的标签和分配审阅人员等。
7. **代码审查和修改**：
   - 团队成员对 Pull Request 中的代码进行审查，并提供反馈和建议。
   - 开发人员根据审阅意见进行修改，并在 Pull Request 中持续交流。
8. **合并 Pull Request**：
   - 当 Pull Request 中的代码经过审阅并达到一定的质量标准后，将其合并到主分支中。
9. **持续集成和持续交付**：
   - 在主分支中的每次提交都会触发 GitLab CI/CD 流水线，自动进行构建、测试和部署。
   - 确保持续集成和持续交付的流程能够保证代码的稳定性和可靠性。
10. **部署到生产环境**：
    - 当新功能经过持续集成和持续交付流程，且通过了所有测试后，可以将代码部署到生产环境中，供最终用户使用。

这些步骤描述了使用 GitLab Flow 进行软件开发的基本流程。通过这种方式，团队可以实现快速迭代、持续交付高质量的代码，并保持代码库的稳定性和可靠性