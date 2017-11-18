# 新人说明
> # Onboarding

> 译者注：本文中的**合作者**指的是已经加入 Node.js 成员的开发者，**新人**便是指新加入的合作者，
而**贡献者**则是指任何一个向 Node.js 进行过贡献的普通 **Contributor**

这篇文档描述了针对新加入合作者的一些培训注意事项。

> This document is an outline of the things we tell new Collaborators at their
onboarding session.

## 新人培训一周前
> ## One week before the onboarding session

* 确认新合作者的 Github 账号开启了两步验证，如果账号没有开启两步验证，则不能给账号授予任何特殊权限，例如合并代码，开启 CI 任务等

> * Confirm that the new Collaborator is using two-factor authentication on their
>   GitHub account. Unless two-factor authentication is enabled, do not give an
>   account elevated privileges such as the ability to land code in the main
>   repository or to start continuous integration (CI) jobs.

## 新人培训 15 分钟前
> ## Fifteen minutes before the onboarding session

* 先将新人加入 [the Collaborators team](https://github.com/orgs/nodejs/teams/collaborators)，和 [the Members team](https://github.com/orgs/nodejs/teams/members)  。
  要注意被加入的账号必须已经开启两步验证
> * Prior to the onboarding session, add the new Collaborator to
  [the Collaborators team](https://github.com/orgs/nodejs/teams/collaborators),
  and to [the Members team](https://github.com/orgs/nodejs/teams/members) if
  they are not already part of it.
  Note that this is the step that gives the account elevated privileges, so
  do not perform this step (or any subsequent steps) unless two-factor
  authentication is enabled on the new Collaborator's GitHub account.

## 新人培训
> ## Onboarding session

* 培训内容包括：
    * [本地设置](#local-setup)
    * [项目目标和价值](#project-goals--values)
    * [管理 issue](#managing-the-issue-tracker)
    * [review PR](#reviewing-prs)
    * [合并 PR](#landing-prs)

> * This session will cover:
>     * [local setup](#local-setup)
>     * [project goals & values](#project-goals--values)
>     * [managing the issue tracker](#managing-the-issue-tracker)
>     * [reviewing PRs](#reviewing-prs)
>     * [landing PRs](#landing-prs)

## 本地设置
> ## Local setup

  * git:
    * 配置 `git config --global --add apply.whitespace fix`
    * 永远从个人 fork 的仓库中发起 PR
      * nodejs/node 仓库中的所有分支只是用于发布
    * [了解如何从上游更新 Node.js](./onboarding-extras.md#updating-nodejs-from-upstream)
    * 为你的每个新 PR 创建新分支
>  * git:
>    * Make sure you have whitespace=fix: `git config --global --add apply.whitespace fix`
>    * Always continue to PR from your own github fork
>      * Branches in the nodejs/node repository are only for release lines
>    * [See "Updating Node.js from Upstream"](./onboarding-extras.md#updating-nodejs-from-upstream)
>    * Make a new branch for each PR you submit.

  * 关于通知：
    * 请注意，如果使用 [https://github.com/notifications](https://github.com/notifications) 或者设置邮箱关注仓库会导致信息爆炸（几天可能就会有数百通知）
  * [webchat.freenode.net](https://webchat.freenode.net/) 上的 `#node-dev` 频道是和其他合作者交流的最佳地点。
    * 如果培训后有任何问题，都可以在这里提问！
    * 并不禁止强制推送 master 分支，但是请写好说明

>  * Notifications:
>    * Use [https://github.com/notifications](https://github.com/notifications) or set up email
>    * Watching the main repo will flood your inbox (several hundred notifications on typical weekdays), so be prepared
>
>  * `#node-dev` on [webchat.freenode.net](https://webchat.freenode.net/) is the best place to interact with the TSC / other Collaborators
>    * If there are any questions after the session, a good place to ask is there!
>    * Presence is not mandatory, but please drop a note there if force-pushing to `master`

目标和价值
> ## Project goals & values

  * 项目属于合作者们集体所有
    * 项目有来自于它的合作者们的目标
  
  * 也有更高层次的目标和价值
    * 关心用户的问题
    * 做一个友善的合作者
    * 最好的结果是，那些向项目提出问题的人，下次还能够再次回来

>  * Collaborators are the collective owners of the project
>    * The project has the goals of its contributors

>  * There are some higher-level goals and values
>    * Empathy towards users matters (this is in part why we onboard people)
>    * Generally: try to be nice to people!
>    * The best outcome is for people who come to our issue tracker to feel like they can come back again.

  * 请遵守我们的[行为准则](https://github.com/nodejs/TSC/blob/master/CODE_OF_CONDUCT.md)，并且督促其他人也遵守准则
  > * We have a [Code of Conduct][] that you are expected to follow *and* hold others accountable to

## 管理 issue
> ## Managing the issue tracker

  * 几乎可以自由处理 issue，如果你觉得一个 issue 应当被关闭，那么请不要犹豫
    * 关闭 issue 的时候，请保持友好！让其他人知道为什么关闭，并且告诉他们这些 issue 或 PRs 如果需要，可以随时被打开

>  * You have (mostly) free rein; don't hesitate to close an issue if you are confident that it should be closed
>    * Be nice about closing issues! Let people know why, and that issues and PRs can be reopened if necessary

  * [**详见标签说明**](./onboarding-extras.md#labels)
    * [机器人](https://github.com/nodejs-github-bot/github-bot)会自动打上子系统的标签（例如：`doc`，`test`，`assert`，`buffer`），这样我们能很方便的知道 PR  修改了哪部分代码。
当然机器人不是完美的。你可以自由的添加相关的、移除不相关的标签。
    * 如果一个问题长时间争论没有结果，可以使用 `tsc-review` 标签
    * `sermver-{minor,major}`:
      * 如果修改会引起不兼容，使用 `sermver-major` 标签
      * 如果使用了 `sermver` 相关的标签，请立即补充评论，说明理由
>  * [**See "Labels"**](./onboarding-extras.md#labels)
>    * There is [a bot](https://github.com/nodejs-github-bot/github-bot) that applies subsystem labels (for example, `doc`, `test`, `assert`, or `buffer`) so that we know what parts of the code base the pull request modifies. It is not perfect, of course. Feel free to apply relevant labels and remove irrelevant labels from pull requests and issues.
>    * Use the `tsc-review` label if a topic is controversial or isn't coming to
>      a conclusion after an extended time.
>    * `semver-{minor,major}`:
>      * If a change has the remote *chance* of breaking something, use the `semver-major` label
>      * When adding a semver label, add a comment explaining why you're adding it. Do it right away so you don't forget!

  * [如何在 issue 中通知别人](./onboarding-extras.md#who-to-cc-in-issues)
    * 随着时间推移，这会变得自然而然
    * 说明里列举了很多团队，根据需要随时添加
>  * [**See "Who to CC in issues"**](./onboarding-extras.md#who-to-cc-in-issues)
>    * This will come more naturally over time
>    * For many of the teams listed there, you can ask to be added if you are interested

      * Some are WGs with some process around adding people, others are only there for notifications

  * 当讨论变得激烈时，你可以请求其他合作者一起关注，在私仓[nodejs/moderation](https://github.com/nodejs/moderation) 中创建 issue
    * 所有的 nodejs 组成员都有权利访问这个仓库（并不仅仅是 node.js 核心开发者），其中的内容不应该被泄露。
    * 详细的说明见[这里](https://github.com/nodejs/TSC/blob/master/Moderation-Policy.md)

>  * When a discussion gets heated, you can request that other Collaborators keep an eye on it by opening an issue at the private [nodejs/moderation](https://github.com/nodejs/moderation) repository.
>    * This is a repository to which all members of the `nodejs` GitHub organization (not just Collaborators on Node.js core) have access. Its contents should not be shared externally.
>    * You can find the full moderation policy [here](https://github.com/nodejs/TSC/blob/master/Moderation-Policy.md).

## 审查代码
> ## Reviewing PRs

  * 主要目的是为了提升代码质量
  * 其次是为了其他人能够成功提交代码
    * 每当有人提交 pr，都是壮大社区的机会 
  * 一次 review 一部分，不要让新贡献者感到挫折
    * 不要过于执着一些很小的性能优化，因为 V8 会经常变化。也许今天这些改动能够带来一些性能提升，但是以后就不一定需要了。
  * 注意，你的观点有很重的分量
  * 可以有一些无关紧要的小修改(Nits)，但是不要因此导致 PR 被延期
    * 当你评论 `Nit: change foo() to bar().` 时，
    * 如果这个小修改会导致 PR 延期，你可以在合并代码时，自己修改

>  * The primary goal is for the codebase to improve.
>  * Secondary (but not far off) is for the person submitting code to succeed.
>      A pull request from a new contributor is an opportunity to grow the
>      community.
>  * Review a bit at a time. Do not overwhelm new contributors.
>    * It is tempting to micro-optimize and make everything about relative
>        performance. Don't succumb to that temptation. We change V8 often.
>        Techniques that provide improved performance today may be unnecessary in
>        the future.
>  * Be aware: Your opinion carries a lot of weight!
>  * Nits (requests for small changes that are not essential) are fine, but try
>    to avoid stalling the pull request.
>    * Note that they are nits when you comment: `Nit: change foo() to bar().`
>    * If they are stalling the pull request, fix them yourself on merge.

  * 等待评论时间
    * 一些重要改动，请等待一段时间，便于其他人有足够的时间回复
    * 对于重要改动，请保留 PR 被打开 48 小时（周末 72 小时） 
    * 如果一个 PR 被弃用，请确认其他人是否介意被你接手（尤其当代码只剩一些小改动需要完善时）
>  * Minimum wait for comments time
>    * There is a minimum waiting time which we try to respect for non-trivial
>        changes, so that people who may have important input in such a
>        distributed project are able to respond.
>    * For non-trivial changes, leave the pull request open for at least 48
>        hours (72 hours on a weekend).
>    * If a pull request is abandoned, check if they'd mind if you took it over
>        (especially if it just has nits left).

  * 赞成修改
    * 合作者通过 Github `approval` 功能，表明他们已经 review 过代码，并赞成 PR 中的修改
    * 一些人喜欢评论 `LGTM`（"Looks Good To Me"）
    * 你可以赞成任何其他合作者的 PR
    * 你不能赞成你自己的 PR

>  * Approving a change
>    * Collaborators indicate that they have reviewed and approve of the
>        the changes in a pull request using Github’s approval interface
>    * Some people like to comment `LGTM` (“Looks Good To Me”)
>    * You have the authority to approve any other collaborator’s work.
>    * You cannot approve your own pull requests.
>    * When explicitly using `Changes requested`, show empathy – comments will
>      usually be addressed even if you don’t use it.
>      * If you do, it is nice if you are available later to check whether your
>        comments have been addressed
>      * If you see that the requested changes have been made, you can clear another collaborator's
>        `Changes requested` review.
>      * Use `Changes requested` to indicate that you are considering some of
>        your comments to block the PR from landing.

  * 哪些应当在 Node.js 内实现：
    * 可以有不同的观点 - 这点有利于众多的合作者一起协作
    * 如果 Node.js 本身需要，那么它属于 Node.js
      * 也就是说，url 模块因为 http 需要，freelist 因为 http 模块等
    * 一些无法在 Node.js 核心之外实现的功能，或者实现起来非常痛苦（例如 `async_hooks` 模块）

>  * What belongs in Node.js:
>    * Opinions vary – it’s good to have a broad collaborator base for that reason!
>    * If Node.js itself needs it (due to historic reasons), then it belongs in Node.js
>      * That is to say, url is there because of http, freelist is there because of http, etc.
>    * Things that cannot be done outside of core, or only with significant pain (for example `async_hooks`)

  * 集成测试：
    * [https://ci.nodejs.org/](https://ci.nodejs.org/)
      * 这个不是自动执行的，需要你手动启动
    * CI 日志已经和 Github 集成在一起，从现在开始试用吧
    * 大多数情况下，一般用 `node-test-pull-request` 启动测试。从这里开始吧！ 

>  * Continuous Integration (CI) Testing:
>    * [https://ci.nodejs.org/](https://ci.nodejs.org/)
>      * It is not automatically run. You need to start it manually.
>    * Log in on CI is integrated with GitHub. Try to log in now!
>    * You will be using `node-test-pull-request` most of the time. Go there now!
>      * Consider bookmarking it: https://ci.nodejs.org/job/node-test-pull-request/
>    * To get to the form to start a job, click on `Build with Parameters`. (If you don't see it, that probably means you are not logged in!) Click it now!
>    * To start CI testing from this screen, you need to fill in two elements on the form:
>      * The `CERTIFY_SAFE` box should be checked. By checking it, you are indicating that you have reviewed the code you are about to test and you are confident that it does not contain any malicious code. (We don't want people hijacking our CI hosts to attack other hosts on the internet, for example!)
>      * The `PR_ID` box should be filled in with the number identifying the pull request containing the code you wish to test. For example, if the URL for the pull request is `https://github.com/nodejs/node/issues/7006`, then put `7006` in the `PR_ID`.
>      * The remaining elements on the form are typically unchanged with the exception of `POST_STATUS_TO_PR`. Check that if you want a CI status indicator to be automatically inserted into the PR.
>    * If you need help with something CI-related:
>      * Use #node-dev (IRC) to talk to other Collaborators.
>      * Use #node-build (IRC) to talk to the Build WG members who maintain the CI infrastructure.
>      * Use the [Build WG repo](https://github.com/nodejs/build) to file issues for the Build WG members who maintain the CI infrastructure.

## 合并 PR
> ## Landing PRs

  * [详见 Landing Pull Requests](https://github.com/nodejs/node/blob/master/COLLABORATOR_GUIDE.md#landing-pull-requests)
  > * [See the Collaborator Guide: Landing Pull Requests](https://github.com/nodejs/node/blob/master/COLLABORATOR_GUIDE.md#landing-pull-requests)

## 练习: 创建一个 PR 将你的名字加入 README
> ## Exercise: Make a PR adding yourself to the README

  * 示例：[https://github.com/nodejs/node/commit/ce986de829457c39257cd205067602e765768fb0](https://github.com/nodejs/node/commit/ce986de829457c39257cd205067602e765768fb0) 
  > * Example: [https://github.com/nodejs/node/commit/ce986de829457c39257cd205067602e765768fb0](https://github.com/nodejs/node/commit/ce986de829457c39257cd205067602e765768fb0)

    * 查看原始的提交信息：`git log ce986de829457c39257cd205067602e765768fb0 -1`
    > * For raw commit message: `git log ce986de829457c39257cd205067602e765768fb0 -1`

  * 合作者按照 Github 用户名字典序排列
  * 还可以添加你的个人代号
  * PR 使用 `doc` 标签
  * 为你的 PR 启动 CI
  * 在一到两个 `Approval` 之后，合并 PR
    * 确保添加了 `PR-URL: <full-pr-url>` 和适当的 `Reviewed-By` 信息
    * 如果你使用 Chrome，[node-review][] 插件会帮你生成这些信息

>  * Collaborators are in alphabetical order by GitHub username.
>  * Optionally, include your personal pronouns.
>  * Label your pull request with the `doc` subsystem label.
>  * Run CI on your PR.
>  * After one or two approvals, land the PR.
>    * Be sure to add the `PR-URL: <full-pr-url>` and appropriate `Reviewed-By:` metadata!
>    * [`core-validate-commit`][] helps a lot with this – install and use it if you can!
>    * If you use Chrome, [`node-review`][] fetches the metadata for you

## 写在最后
> ## Final notes

  * 不要担心犯错：每个人都会犯错，有太多东西需要花时间消化
  * 几乎任何你可能犯的错，都可以被修复或回滚
  * 老司机们相信你，并且感激你的帮助

>  * Don't worry about making mistakes: everybody makes them, there's a lot to internalize and that takes time (and we recognize that!)
>  * Almost any mistake you could make can be fixed or reverted.
>  * The existing Collaborators trust you and are grateful for your help!

  * 其他值得关注的项目：
  > * Other repositories:

    * [https://github.com/nodejs/TSC](https://github.com/nodejs/TSC)
    * [https://github.com/nodejs/build](https://github.com/nodejs/build)
    * [https://github.com/nodejs/nodejs.org](https://github.com/nodejs/nodejs.org)
    * [https://github.com/nodejs/readable-stream](https://github.com/nodejs/readable-stream)
    * [https://github.com/nodejs/LTS](https://github.com/nodejs/LTS)
    * [https://github.com/nodejs/citgm](https://github.com/nodejs/citgm)

  * Node.js 基金会会定期举办峰会，召集 Node.js 项目的活跃合作者，我们将会面对面的交流我们的工作。基金会有旅行基金提供给参会者，包括住宿、交通、签证费用等，详情见 [summit](https://github.com/nodejs/summit)

>  * The Node.js Foundation hosts regular summits for active contributors to the Node.js
>    project, where we have face-to-face discussion about our work on the project.
>    The foundation has travel funds to cover participants' expenses including
>    accommodation, transportation, visa fees etc. if needed. Check out the
>    [summit](https://github.com/nodejs/summit) repository for details.

[Code of Conduct]: https://github.com/nodejs/TSC/blob/master/CODE_OF_CONDUCT.md
[`core-validate-commit`]: https://github.com/evanlucas/core-validate-commit
[`node-review`]: https://github.com/evanlucas/node-review
