# Debian 入库整改 Skill

这是一个面向 Debian 官方仓库入库前整改的 Codex/AGENTS.md 初稿，用于辅助检查自研 Debian 源码包是否适合提交到 Debian mentors、RFS 或 sponsor 审核流程。

当前版本重点覆盖：

- `debian/control`、`debian/changelog`、`debian/copyright`、`debian/rules` 等核心打包文件审查
- Lintian、debuild、uscan、reprotest 等工具驱动的验证流程
- shared library、maintainer scripts、systemd、D-Bus、desktop、AppStream 等常见入库风险点
- UKUI / Kylin / openKylin 软件包面向 Debian unstable 时的差异检查

## 使用方式

把 `AGENTS.md` 放到 Debian 源码包根目录，然后在该目录启动 Codex：

```bash
cp AGENTS.md /path/to/package/
cd /path/to/package
codex
```

推荐第一次只做静态审查，不要马上修改文件。可直接使用下方“首次静态审查”提示词。

## 推荐提示词

下面这些提示词可以直接复制到 Codex 对话中使用。建议第一次审查只生成报告，不直接修改源码；确认问题后再进入整改阶段。

### 首次静态审查

```text
请按照 AGENTS.md 的规则，对当前 Debian 源码包做第一次入库级审查。
要求：
1. 不要直接修改文件；
2. 先读取 debian/ 目录和主要构建文件；
3. 输出“必须修复”“建议修复”“可以暂缓”三类问题；
4. 对每个问题给出原因和最小修改建议；
5. 特别关注 Debian Policy、lintian、debuild、uscan、copyright、shared library、maintainer scripts；
6. 如果发现 Ubuntu/Kylin/PPA 专用写法，请单独列出来。
```

### 首次审查并生成报告文件

```text
请按照 AGENTS.md 的规则，对当前 Debian 源码包做第一次入库级审查。
要求：
1. 不要修改源码和 debian/ 打包文件；
2. 请将本次审查结果写入当前目录下的 DEBIAN_REVIEW_REPORT.md；
3. 先读取 debian/ 目录和主要构建文件；
4. 输出并写入“必须修复”“建议修复”“可以暂缓”三类问题；
5. 对每个问题给出原因和最小修改建议；
6. 特别关注 Debian Policy、lintian、debuild、uscan、copyright、shared library、maintainer scripts；
7. 如果发现 Ubuntu/Kylin/PPA 专用写法，请单独列出来；
8. 报告中请包含：审查日期、包名、A/B/C/D 分级、必须修复项、建议修复项、可以暂缓项、下一步建议；
9. 如果 DEBIAN_REVIEW_REPORT.md 已存在，请保留历史内容，并以新的审查日期追加一节，不要覆盖旧记录。
```

### UKUI / Kylin / openKylin 包审查

```text
请按照 AGENTS.md 的规则，对当前 UKUI / Kylin / openKylin Debian 源码包做入库级审查。
要求：
1. 不要直接修改文件，先输出审查报告；
2. 重点检查 debian/control 中 Maintainer 是否为 kylin Team <team+kylin@tracker.debian.org>；
3. 重点检查 debian/watch 是否使用 openKylin / Gitee release 模板，并且只替换了对应仓库名和源码包名；
4. 检查是否残留 Ubuntu / Kylin / PPA 专用版本号、路径、服务、主题或依赖；
5. 检查 debian/copyright 是否覆盖 Kylin 品牌资源、图标、字体、第三方代码和 embedded copy；
6. 检查 shared library、maintainer scripts、systemd、D-Bus、desktop、AppStream 是否符合 Debian unstable 入库要求；
7. 按“必须修复”“建议修复”“可以暂缓”输出，并给出最小修改方案；
8. 最后给出 A/B/C/D 分级。
```

### 开始整改

```text
请根据刚才的 Debian 入库整改报告，优先修复“必须修复”部分。
要求：
1. 每次只做一类相关修改；
2. 修改后说明改了哪些文件；
3. 给出建议的 git diff 检查点；
4. 给出下一步应该运行的验证命令；
5. 不要添加没有充分理由的 lintian override。
```

### 按报告继续整改

```text
请读取当前目录下的 DEBIAN_REVIEW_REPORT.md，并根据报告继续整改。
要求：
1. 优先处理“必须修复”部分；
2. 每次只做一类相关修改；
3. 不要修改与当前问题无关的文件；
4. 修改后更新 DEBIAN_REVIEW_REPORT.md 中对应问题的状态；
5. 给出本轮修改涉及的文件、建议检查的 git diff 命令和下一步验证命令；
6. 不要添加没有充分理由的 lintian override。
```

### 分析检查日志

```text
下面是 lintian / debuild / uscan / reprotest 日志。
请按照 Debian sponsor 审核标准分析：
1. 哪些是必须修复；
2. 哪些可以解释或 override；
3. 哪些是环境问题；
4. 每个问题对应的最小修改方案；
5. 修改后应该重新运行哪些命令。
```

### 跑验证前制定命令顺序

```text
请根据当前包的状态，按照 AGENTS.md 的规则给出验证顺序。
要求：
1. 先说明哪些命令应该先跑，哪些命令适合第二轮再跑；
2. 覆盖 fakeroot debian/rules clean、dpkg-source --build、debuild、lintian、uscan、reprotest；
3. 如果某些工具当前环境可能缺失，请说明缺失时的处理方式；
4. 不要修改文件，只输出建议命令和每条命令的目的。
```

### RFS 准备

```text
请按照 Debian RFS / sponsor 审核视角，检查当前包是否已经适合请求赞助上传。
请重点检查：
1. changelog 是否关闭 ITP bug；
2. debian/control 是否适合 Debian；
3. debian/copyright 是否完整；
4. lintian 是否还有 error/warning；
5. debuild/lintian/uscan 是否通过；
6. mentors 上传前还缺什么；
7. RFS 邮件应该怎么写。
```

## 批量审查流程

如果需要审查一批包，例如 `/home/zp/code/ai` 下的多个 Debian 源码包，建议先做一轮快速分级，再集中整改优先级最高、入库希望最大的包。

第一轮只判断包的状态，不直接修改源码：

1. 给每个包复制或软链接 `AGENTS.md`。
2. 在包根目录运行 Codex，并使用上面的首次审查提示词。
3. 把审查结果记录到 `REVIEW_STATUS.md`。
4. 根据阻塞程度给包打 A/B/C/D 标签。
5. 优先处理 A 类和 B 类包，C/D 类先记录阻塞原因，暂不消耗过多整改时间。

建议分级标准：

| 标签 | 含义 | 建议处理方式 |
|---|---|---|
| A | 基本可入库，只需小修 | 优先整改，尽快跑 debuild、lintian、uscan |
| B | 可以整改，但需要一轮构建、版权或依赖处理 | 第二优先级，先解决明确阻塞项 |
| C | 阻塞较大，license、DFSG、embedded copy、架构拆包等有硬问题 | 先确认是否值得投入，必要时拆分专题处理 |
| D | 暂不适合 Debian 入库 | 记录原因，暂缓推进 |

## 审查记录

仓库提供 `REVIEW_STATUS.md` 作为批量审查记录模板。建议每审查一次都记录：

- 包名和源码路径
- 审查日期
- 当前分级
- 最大阻塞项
- debuild、lintian、uscan 状态
- 下一步最小整改动作

这样可以避免重复审查，也方便后续按优先级推进。

## 文件说明

- `AGENTS.md`: 核心 Skill 指令，包含审查范围、执行命令、整改优先级和输出格式。
- `README.md`: 仓库说明和快速使用方式。
- `REVIEW_STATUS.md`: 批量审查记录模板，用于追踪每个包的状态、阻塞项和下一步动作。

## 当前状态

这是初稿版本，后续计划继续完善：

- 增加更细的 Debian Policy / Lintian 标签映射
- 增加常见包类型的专项 checklist
- 增加 RFS 邮件、ITP bug、mentors 上传前模板
- 根据真实 sponsor 反馈持续调整审查规则

## 说明

这个 Skill 旨在辅助发现问题和组织整改思路，不能替代 Debian Policy、Developer's Reference、Maintainer Guide、Lintian、debuild、uscan 等官方文档和工具结果。
