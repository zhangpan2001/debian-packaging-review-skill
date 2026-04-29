# Debian 入库整改 Skill

这是一个面向 Debian 官方仓库入库前整改的 Codex/AGENTS.md 初稿，用于辅助检查自研 Debian 源码包是否适合提交到 Debian mentors、RFS 或 sponsor 审核流程。

当前版本重点覆盖：

- `debian/control`、`debian/changelog`、`debian/copyright`、`debian/rules` 等核心打包文件审查
- Lintian、sbuild、piuparts、autopkgtest、reprotest 等工具驱动的验证流程
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

### 首次静态审查

```text
请按照 AGENTS.md 的规则，对当前 Debian 源码包做第一次入库级审查。
要求：
1. 不要直接修改文件；
2. 先读取 debian/ 目录和主要构建文件；
3. 输出“必须修复”“建议修复”“可以暂缓”三类问题；
4. 对每个问题给出原因和最小修改建议；
5. 特别关注 Debian Policy、lintian、sbuild、piuparts、autopkgtest、copyright、shared library、maintainer scripts；
6. 如果发现 Ubuntu/Kylin/PPA 专用写法，请单独列出来。
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

### 分析检查日志

```text
下面是 lintian / sbuild / piuparts / autopkgtest 日志。
请按照 Debian sponsor 审核标准分析：
1. 哪些是必须修复；
2. 哪些可以解释或 override；
3. 哪些是环境问题；
4. 每个问题对应的最小修改方案；
5. 修改后应该重新运行哪些命令。
```

### RFS 准备

```text
请按照 Debian RFS / sponsor 审核视角，检查当前包是否已经适合请求赞助上传。
请重点检查：
1. changelog 是否关闭 ITP bug；
2. debian/control 是否适合 Debian；
3. debian/copyright 是否完整；
4. lintian 是否还有 error/warning；
5. sbuild/piuparts/autopkgtest 是否通过；
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
| A | 基本可入库，只需小修 | 优先整改，尽快跑 lintian、sbuild、piuparts、autopkgtest |
| B | 可以整改，但需要一轮构建、版权或依赖处理 | 第二优先级，先解决明确阻塞项 |
| C | 阻塞较大，license、DFSG、embedded copy、架构拆包等有硬问题 | 先确认是否值得投入，必要时拆分专题处理 |
| D | 暂不适合 Debian 入库 | 记录原因，暂缓推进 |

## 审查记录

仓库提供 `REVIEW_STATUS.md` 作为批量审查记录模板。建议每审查一次都记录：

- 包名和源码路径
- 审查日期
- 当前分级
- 最大阻塞项
- lintian、sbuild、piuparts、autopkgtest 状态
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

这个 Skill 旨在辅助发现问题和组织整改思路，不能替代 Debian Policy、Developer's Reference、Maintainer Guide、Lintian、sbuild、piuparts、autopkgtest 等官方文档和工具结果。
