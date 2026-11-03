# henry · ghpub CI 演示库

本仓库是 **ghpub**（零依赖 GitHub 发布 CLI）的 **GitHub Actions 自动发布演示库**。

> 本仓库不含任何业务源码。原业务源码「个人办公智能体」已迁移至独立仓库 `henrychen8888/office-agent`。henry 仅用于演示「打 `v*` tag 自动发版」这条 CI 链路。

## 演示内容

- 推送 `v*` tag 到本仓库 → GitHub Actions 自动执行 `ghpub`，创建 Release 并把制品作为 **Release asset** 上传（而非仓库文件）。
- CI 零配置：`secrets.GITHUB_TOKEN` 直接驱动，无需任何密钥文件。
- 演示「智能发布」：仓库已存在 → 跳过建仓 → 直接 push + 打 tag + 发 Release。

## 目录

```
.github/
  ghpub          # 随仓库打包进 CI 的 ghpub CLI（单文件、零依赖）
  workflows/
    publish.yml  # 自动发布工作流（需本地 git push，见下方说明）
README.md        # 本文件
```

## 触发一次自动发版

```bash
git clone <本仓库>
cd henry
git tag v1.0.0
git push origin v1.0.0     # 推送 v* tag 即触发 Actions
```

运行完成后，仓库 Releases 页会出现对应 Release 及附件。

## 为什么 workflow 不在仓库里

GitHub 安全限制：**REST API 禁止写入 `.github/workflows/`**，因此 `publish.yml` 必须由本地 `git push` 提交（见 `publish-workflow-henry.sh`），不能通过 API 上传。
