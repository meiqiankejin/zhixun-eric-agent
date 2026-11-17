# 智训引擎 Agent 源码仓库

完整源码见本仓库中的 **`zhixun-eric-agent-v0.1.0-source-code.zip`**，下载解压即可查看所有源码目录。公开包包含78份源码、配置及说明，其中71份文件与交付镜像原构建上下文逐字节一致；不含训练数据、模型权重、API密钥或本机运行日志。

这是 `zhixun-eric-agent:0.1.0` 的源码交付。此处的源码ZIP与队伍另外提交的约30MB完整构建资源包不同；源码ZIP不包含竞赛数据或检查点。ZIP中 `docs/IMAGE-CORRESPONDENCE.json` 记录镜像、源码与资源的对应摘要。

仓库目前通过GitHub网页发布源码归档，GitHub页面不直接展开ZIP内的目录。以下目录说明均指解压后的内容。

---

# 智训引擎 Agent

用于气体分类、轴承剩余寿命和发动机剩余循环数任务的受控续训 Agent。
本仓库对应 `zhixun-eric-agent:0.1.0` 镜像中的源码，提供千问决策、受审核代码模板/参数动作、隔离续训、宿主评估、模型导出和过程记录。

## 当前状态

- Linux/amd64，Python 3.12.14，NumPy 1.26.4；CPU 续训。
- 驱动模型为 PyTRiO 提供的 `Qwen/Qwen3.8-27B` 官方基础模型；没有千问微调权重或 Sampler。
- A 使用受审核的加权交叉熵模板；B/C 使用受约束参数控制器。
- 原官方检查点继承权重、Adam 和缩放状态；公开 validation 不用于梯度更新。
- 镜像已在本地完成真实千问 A/B/C 闭环，并通过独立脚本自查；这是开发功能验证，不是官方比赛或隐藏集成绩。
- 正式平台启动、动态父检查点、挂载、轮次/finish 和云端记录接口尚未对齐，不能承诺上传即评测成功。

## 阅读代码

- `context/app/linux_entry.py`：Linux 真实控制入口及凭据预检。
- `context/app/linux_offline_entry.py`：完整性核验、无标签预测和开发计划冻结。
- `context/app/outputs/industrial-maintenance-demo/`：控制器、工具、训练监督器和哈希记录链。
- `context/app/inputs/official-platform-2026-11-11/`：参考任务说明、配置和官方模型/训练/预测源码。
- `context/app/outputs/unified-entry-v1-existing-model-exports-final/`：历史保留模型对应预测源码和配置；权重不在此公开仓库。

## 公开范围与复现

本仓库只发布源码、配置、原任务文档和摘要清单，不发布竞赛数据、检查点、运行日志、API 密钥、离线凭据或依赖二进制。原镜像和构建源码 ZIP 由队伍单独提交；不要把仓库克隆直接当作完整运行环境。

取得队伍的 `zhixun-eric-agent-0.1.0-source.zip` 后，可用以下工具核验并重建完整构建上下文：

```sh
python tools/prepare_context.py --source-zip /path/to/zhixun-eric-agent-0.1.0-source.zip --output build/context
docker build --platform linux/amd64 --network=none -t zhixun-eric-agent:0.1.0 build/context
docker save -o zhixun-eric-agent-0.1.0-linux-amd64.tar zhixun-eric-agent:0.1.0
```

构建后默认执行 `check`，不会自行训练或联网。开发真实训练必须先固定计划，再显式执行 `train-live --live`；密钥仅由调用方凭据管理工具通过 stdin 传入。不要将密钥写入 Git、Dockerfile、命令行或记录。
重建可验证源码/文件一致性，不保证不同 Docker 构建器的镜像归档逐字节一致。

## 已验证范围

同一 Linux 镜像的真实运行曾使用 3 次拟合、4 次请求、7664 个已报告 Token；仅 A 有 1 次完整的真实提案—实验—决定循环，B/C 各 1 次真实提案。不能由此声称满足正式至少 3 次历史循环或证明自改进有效。
重新打包的离线验收包含默认启动、三任务各 2 条无标签预测，共 4 个断网容器；没有新增训练、API 请求或平台试运行。

详细文件对应关系见 `docs/IMAGE-CORRESPONDENCE.json`，接口边界见 `docs/INTERFACE-STATUS.md`，来源见 `SOURCE_NOTICE.md`。
Codex 自查，未经 Claude 独立复核。
