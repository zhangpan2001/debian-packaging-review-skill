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

推荐首次输入：

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

## 文件说明

- `AGENTS.md`: 核心 Skill 指令，包含审查范围、执行命令、整改优先级和输出格式。
- `README.md`: 仓库说明和快速使用方式。

## 当前状态

这是初稿版本，后续计划继续完善：

- 增加更细的 Debian Policy / Lintian 标签映射
- 增加常见包类型的专项 checklist
- 增加 RFS 邮件、ITP bug、mentors 上传前模板
- 根据真实 sponsor 反馈持续调整审查规则

## 说明

这个 Skill 旨在辅助发现问题和组织整改思路，不能替代 Debian Policy、Developer's Reference、Maintainer Guide、Lintian、sbuild、piuparts、autopkgtest 等官方文档和工具结果。
