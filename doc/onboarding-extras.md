# 新手其他需要了解的信息

> # Additional Onboarding Information

## issues 中呼叫谁？
> ## Who to CC in issues

| Subsystem                             | Maintainers                                                           |
| ---                                   | ---                                                                   |
| `benchmark/*`                         | @nodejs/benchmarking, @mscdex                                         |
| `bootstrap_node.js`                   | @fishrock123                                                          |
| `doc/*`, `*.md`                       | @nodejs/documentation                                                 |
| `lib/assert`                          | @nodejs/testing                                                       |
| `lib/buffer`                          | @nodejs/buffer                                                        |
| `lib/child_process`                   | @bnoordhuis, @cjihrig                                                 |
| `lib/cluster`                         | @bnoordhuis, @cjihrig, @mcollina                                      |
| `lib/{crypto,tls,https}`              | @nodejs/crypto                                                        |
| `lib/dgram`                           | @cjihrig, @mcollina                                                   |
| `lib/domains`                         | @misterdjules                                                         |
| `lib/fs`, `src/{fs,file}`             | @nodejs/fs                                                            |
| `lib/{_}http{*}`                      | @nodejs/http                                                          |
| `lib/inspector.js`, `src/inspector_*` | @nodejs/v8-inspector                                                  |
| `lib/internal/url`, `src/node_url`    | @nodejs/url                                                           |
| `lib/net`                             | @bnoordhuis, @indutny, @nodejs/streams                                |
| `lib/repl`                            | @addaleax, @fishrock123                                               |
| `lib/{_}stream{*}`                    | @nodejs/streams                                                       |
| `lib/timers`                          | @fishrock123, @misterdjules                                           |
| `lib/util`                            | @bnoordhuis, @cjihrig, @evanlucas                                     |
| `lib/zlib`                            | @addaleax, @bnoordhuis, @indutny                                      |
| `src/async-wrap.*`                    | @nodejs/async\_hooks                                                  |
| `src/node_api.*`                      | @nodejs/n-api                                                         |
| `src/node_crypto.*`                   | @nodejs/crypto                                                        |
| `test/*`                              | @nodejs/testing                                                       |
| `tools/eslint`, `.eslintrc`           | @not-an-aardvark, @silverwind, @trott                                 |
| async\_hooks                          | @nodejs/async\_hooks for bugs/reviews (+ @nodejs/diagnostics for API) |
| build                                 | @nodejs/build                                                         |
| GYP                                   | @nodejs/gyp                                                           |
| performance                           | @nodejs/performance                                                   |
| platform specific                     | @nodejs/platform-{aix,arm,freebsd,macos,ppc,smartos,s390,windows}     |
| python code                           | @nodejs/python                                                        |
| upgrading c-ares                      | @jbergstroem                                                          |
| upgrading http-parser                 | @jbergstroem, @nodejs/http                                            |
| upgrading libuv                       | @saghul                                                               |
| upgrading npm                         | @fishrock123, @MylesBorins                                            |
| upgrading V8                          | @nodejs/v8, @nodejs/post-mortem                                       |

当一些情况需要特别注意时，或者存在争议，或者引起重大变更时，`@nodejs/tsc`

> When things need extra attention, are controversial, or `semver-major`: @nodejs/tsc

如果对一个文件有疑惑，你不知道应该呼叫谁时，可以使用 `git shortlog -n -s <file>` 命令，也许会有帮助。
> If you cannot find who to cc for a file, `git shortlog -n -s <file>` may help.

## 正确使用标签
> ## Labels

### 子模块标签

### By Subsystem

我们一般情况下通过子模块的概念，排序 issues，这样的话，我们知道新修改的代码涉及仓库的哪部分。
> We generally sort issues by a concept of "subsystem" so that we know what part(s) of the codebase it touches.

**哪些是子模块**: 
**Subsystems generally are**:

> * `lib/*.js`
> * `doc`, `build`, `tools`, `test`, `deps`, `lib / src` (special), and there may be others.
> * `meta` for anything non-code (process) related

一个 issue / PR 可能涉及多个子系统
> There may be more than one subsystem valid for any particular issue / PR.

### 通用标签
> ### General

请合理使用如下标签：
> Please use these when possible / appropriate

* `confirmed-bug` - 你确信这是一个 bug 时
* `discuss` - 需要大家一起讨论时
* `feature request` - 你希望 Node.js 能够实现/支持的一个新特性时（一般不用于 PR）
* `good first issue` - 适合新人处理的 issue

> * `confirmed-bug` - Bugs you have verified exist
> * `discuss` - Things that need larger discussion
> * `feature request` - Any issue that requests a new feature (usually not PRs)
> * `good first issue` - Issues suitable for newcomers to process

--

 * 版本号相关标签选择 `semver-{minor,major}`
   * 保守一点 – 如果一个修改很可能会导致不兼容, 使用 `semver-major`
   * 当使用一个版本变动标签时，请补充说明为什么使用版本变动相关标签
   * 次版本(minor) vs. 修订(patch): 一般当添加一个新方法或添加新的文档说明时使用 `minor`
   * 主版本 vs. 其他: 如果在当前版本使用上一个版本的测试，运行通过了，**也许**应该使用 `minor` 或 `patch`
   * 使用如下命令进行兼容性检测 ([完整代码](https://gist.github.com/chrisdickinson/ba532fa0e4e243fb7b44)):
   ```sh
   git checkout $(git show -s --pretty='%T' $(git show-ref -d $(git describe --abbrev=0) | tail -n1 | awk '{print $1}')) -- test; make -j4 test
   ```

> * `semver-{minor,major}`
>   * be conservative – that is, if a change has the remote *chance* of breaking something, go for semver-major
>   * when adding a semver label, add a comment explaining why you're adding it
>   * minor vs. patch: roughly: "does it add a new method / does it add a new section to the docs"
>   * major vs. everything else: run last versions tests against this version, if they pass, **probably** minor or patch
>   * A breaking change helper ([full source](https://gist.github.com/chrisdickinson/ba532fa0e4e243fb7b44)):
>   ```sh
>   git checkout $(git show -s --pretty='%T' $(git show-ref -d $(git describe --abbrev=0) | tail -n1 | awk '{print $1}')) -- test; make -j4 test
>   ```

### 长期维护版本相关的标签

> ### LTS/Version labels

我们通过标签来跟踪一个修改应当合并到哪些分支中去：
> We use labels to keep track of which branches a commit should land on:

* `dont-land-on-v?.x`
  * 用于说明这些修改不能被合并到某个分支中
  * 也用于说明将这些修改迁移到某个分支没有什么价值
* `land-on-v?.x`
  * 版本发行者使用这些标签来标记一个 PR 将会被如期合并到一个长期维护的版本中
  * 适用于将一个 PR 中的修改迁移到一些旧版本中去
* `backport-requested-v?.x`
  * 用于说明一个 PR 需要手动迁移修改到其他版本中
  * 特别地，当一个 PR 的修改无法直接应用其他版本时
  * 将会被替换为 `dont-land-on-v?.x` 或 `backported-to-v?.x`
* `backported-to-v?.x`
  * 适用于一个迁移修改的 PR 已经被合并时
* `lts-watch-v?.x`
  * 用于如果一个 PR 需要长期维护工作组评估是否应该被合并到长期维护版本中
  * 不意味着这个 PR 一定会被采取任何行动，但是可能会作为一个信息反馈给其他人
* `lts-agenda`
  * 用于需要长期维护工作组讨论时
  * 例如一个次版本更新是否需要发布一个长期维护版本
* `v?.x`
  * 当修改不是合并到 `master` 分支，而是其他的 `v?.x-staging` 分支时，会被自动添加

> * `dont-land-on-v?.x`
>   * For changes that do not apply to a certain release line
>   * Also used when the work of backporting a change outweighs the benefits
> * `land-on-v?.x`
>   * Used by releasers to mark a PR as scheduled for inclusion in an LTS release
>   * Applied to the original PR for clean cherry-picks, to the backport PR otherwise
> * `backport-requested-v?.x`
>   * Used to indicate that a PR needs a manual backport to a branch in order to land the changes on that branch
>   * Typically applied by a releaser when the PR does not apply cleanly or it breaks the tests after applying
>   * Will be replaced by either `dont-land-on-v?.x` or `backported-to-v?.x`
> * `backported-to-v?.x`
>   * Applied to PRs for which a backport PR has been merged
> * `lts-watch-v?.x`
>   * Applied to PRs which the LTS working group should consider including in a LTS release
>   * Does not indicate that any specific action will be taken, but can be effective as messaging to non-collaborators
> * `lts-agenda`
>   * For things that need discussion by the LTS working group
>   * (for example semver-minor changes that need or should go into an LTS release)
> * `v?.x`
>   * Automatically applied to changes that do not target `master` but rather the `v?.x-staging` branch

一旦一个版本线进入维护模式，这些标签将不会再被用到，除非是特别重要的 bug 修复。

> Once a release line enters maintenance mode, the corresponding labels do not
need to be attached anymore, as only important bugfixes will be included.

### 其他标签
> ### Other Labels

 * 操作系统标签
   * `macos`, `windows`, `smartos`, `aix`
   * 没有 `linux` 标签，默认是 `linux`
 * 架构标签
   * `arm`, `mips`, `s390`, `ppc`
   * 没有 `x86{_64}` 相关标签, 也是默认的

> * Operating system labels
>   * `macos`, `windows`, `smartos`, `aix`
>   * No linux, linux is the implied default
> * Architecture labels
>   * `arm`, `mips`, `s390`, `ppc`
>   * No x86{_64}, since that is the implied default

## 从上游更新 Node.js
> ## Updating Node.js from Upstream

* `git remote add upstream git://github.com/nodejs/node.git`

从 nodejs/node 仓库更新:
* `git checkout master`
* `git remote update -p` OR `git fetch --all` (I prefer the former)
* `git merge --ff-only upstream/master` (or `REMOTENAME/BRANCH`)

## 最佳实践
> ## best practices

* 经常提交，然后开启一个新 PR
* 创建新 PR 前，应该多花时间写描述
  * 每多花点时间写一个很棒的描述，便会节约他人数倍的时间去理解你的代码
* 通常最终会将你的多个 commit 合并为一个 commit

> * commit often, out to your github fork (origin), open a PR
> * when making PRs make sure to spend time on the description:
>   * every moment you spend writing a good description quarters the amount of time it takes to understand your code.
> * usually prefer to only squash at the *end* of your work, depends on the change
