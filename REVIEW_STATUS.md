# Debian 包审查记录

这个文件用于记录每个 Debian 源码包的审查状态。建议每完成一次静态审查、构建验证或整改复查后都更新一次，避免重复劳动，并方便按优先级推进。

## 分级标准

| 标签 | 含义 | 建议处理方式 |
|---|---|---|
| A | 基本可入库，只需小修 | 优先整改，尽快跑 lintian、sbuild、piuparts、autopkgtest |
| B | 可以整改，但需要一轮构建、版权或依赖处理 | 第二优先级，先解决明确阻塞项 |
| C | 阻塞较大，license、DFSG、embedded copy、架构拆包等有硬问题 | 先确认是否值得投入，必要时拆分专题处理 |
| D | 暂不适合 Debian 入库 | 记录原因，暂缓推进 |

## 总览

| 包名 | 路径 | 最近审查日期 | 分级 | 最大阻塞项 | lintian | sbuild | piuparts | autopkgtest | 下一步 |
|---|---|---|---|---|---|---|---|---|---|
| example | `/home/zp/code/ai/example` | YYYY-MM-DD | B | `debian/copyright` 不完整 | 未跑 | 未跑 | 未跑 | 未跑 | 补 DEP-5 后重新审查 |

## 单包记录模板

### package-name

- 路径：
- 最近审查日期：
- 当前分级：
- 当前结论：
- 最大阻塞项：
- 是否发现 Ubuntu/Kylin/PPA 专用写法：

#### 必须修复

| 编号 | 问题 | 文件 | 原因 | 最小修改建议 | 状态 |
|---|---|---|---|---|---|
| 1 |  |  |  |  | 未处理 |

#### 建议修复

| 编号 | 问题 | 文件 | 原因 | 最小修改建议 | 状态 |
|---|---|---|---|---|---|
| 1 |  |  |  |  | 未处理 |

#### 可以暂缓

| 编号 | 问题 | 文件 | 原因 |
|---|---|---|---|
| 1 |  |  |  |

#### 验证状态

| 检查项 | 状态 | 日志或说明 |
|---|---|---|
| `fakeroot debian/rules clean` | 未跑 |  |
| `dpkg-source --build .` | 未跑 |  |
| `debuild -us -uc` | 未跑 |  |
| `lintian -EvIL +pedantic ../*.changes` | 未跑 |  |
| `uscan --verbose --no-download` | 未跑 |  |
| `sbuild --no-clean-source --arch-any --arch-all` | 未跑 |  |
| `piuparts ../*.deb` | 未跑 |  |
| `autopkgtest ../*.dsc -- null` | 未跑 |  |

#### 下一步

- 
