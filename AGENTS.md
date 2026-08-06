# 仓库协作指南

## 项目概览

本仓库维护一个通用 Kubernetes Helm Chart，Chart 根目录为 charts/。模板位于 charts/templates/，默认配置位于 charts/values.yaml，发布工作流位于 .github/workflows/发布.yaml。

## 修改原则

- 优先做最小、可验证的改动；不要顺带重构或修改无关配置。
- 新增可配置项时，同时更新 charts/values.yaml 中的默认值与注释，并在模板中通过 Helm 条件或默认值保持向后兼容。
- 保持 Kubernetes 资源名称、标签选择器和 Deployment 的 selector/pod labels 一致；变更 selector 前确认升级兼容性。
- 对 Secret、镜像凭据和环境变量来源保持谨慎，禁止在仓库中写入真实凭据、令牌或私钥。
- 修改发布相关内容时，注意 GitHub Actions 使用 helm/chart-releaser-action 从仓库根目录发现 Chart。

## 格式规范

- 遵循 .editorconfig：UTF-8、LF 换行、去除行尾空白；YAML 使用 2 空格缩进。
- Helm 模板使用标准 Go template 语法，保持现有缩进和空白控制风格。
- 配置字段命名应清晰且稳定；避免无必要地重命名已有 values 字段。

## 验证

修改 Chart 后，至少运行与改动相关的命令：

    rtk helm lint charts
    rtk helm template deploy charts
    rtk git diff --check

如验证命令因本机未安装 Helm 而无法执行，在交付说明中明确说明。

## 提交前检查

- 使用 rtk git status --short 和 rtk git diff --check 确认仅包含预期改动。
- 不要提交生成的 Chart 包、临时渲染文件或本地凭据。
