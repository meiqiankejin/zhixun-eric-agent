# 智训引擎 Agent — 当前 v0.2.0 / SDK 1.0.2

当前源码见 [zhixun-eric-agent-v0.2.0-sdk102-source-code.zip](zhixun-eric-agent-v0.2.0-sdk102-source-code.zip)。本仓库以源码归档发布，下载解压后可查看完整选手代码。
对应镜像 `zhixun-eric-agent:0.2.0`，Linux/amd64、Python3.12.14、NumPy1.26.4。

本版已实现 `contest.harness.workspace.v2` 的 `/app/harness.py describe/train/predict`，
千问仅通过 `host_gateway_rpc` 调用；父模型、数据、状态与选中模型分别来自平台挂载。
没有把开发版的固定随包父模型、训练数据、个人API客户端或密钥带入本镜像。

已验证SDK原样15项离线测试在Windows和Linux通过，A/B/C宿主替身交互、真实Linux续训、失败处理、状态与历史续接、逐行预测通过。
千问回复是本地替身，尚未运行真实平台网关或消耗平台试运行次数；这些结果不等于官方验收、隐藏集收益或自改进证明。
每轮最多1候选、3epoch/2048步、4次模型请求；A为3个审核模板，B/C为受限配方。
只支持官方 `contest.cpu-mlp.v1` 检查点及本项目同语义模板；不会执行未知父代码或用随包权重代替挂载父模型。

归档内 `context/app/` 为完整运行代码（6文件），`context/Dockerfile` 与锁文件为构建配置；
`docs/IMAGE-RECEIPT.json` 含镜像、归档及源码哈希对应，`verification/` 为本地宿主替身核对工具。
核对工具依赖本项目目录及另行取得的官方公开包，不是官方平台测试程序。
公开源码不含依赖wheel；离线重建需队伍交付的完整构建ZIP，或取得相同SHA256的NumPy和官方SDKwheel。

镜像tar SHA256：`cc0128032c5d64db3014689765421d470580a0bd27d5ba48f59a7d1156d42d80`；镜像ID：`sha256:aa85c3e4a6c999ddb0af0cf62a1ddcebcde6655ae6433691fbdaca36c4ae0641`。
来源与使用边界见 [SOURCE_NOTICE.md](SOURCE_NOTICE.md)。Codex自查，未经Claude复核。

旧 `zhixun-eric-agent-v0.1.0-source-code.zip` 是SDK适配前开发版，保留为历史资料；上传新镜像时应核对v0.2.0归档。
