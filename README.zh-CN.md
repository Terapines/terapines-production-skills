# 🌲 Terapines Production Skills

[English](README.md) | 简体中文

帮助 AI agent 通过命令行使用 Terapines 产品的 skills。
用自然语言描述目标，agent 即可参考相应 skill 检查环境、选择产品命令并检查结果。

本仓库提供 agent 使用的操作指南。产品安装器和下载文件由各 skill 中说明的官方产品渠道提供。

## ✨ 可用 skills

| Skill | 用途 |
| --- | --- |
| [product-manager-next](skills/product-manager-next/SKILL.md) | 提供 Product Manager Next 自身安装/升级、环境检查、产品安装/卸载、账号登录/登出、在线激活、离线 license 导入、远程 LMD 配置和 Product Manager Next 操作故障诊断的指导。 |

Product Manager Next（`product-manager-next`）负责管理 Terapines 5.x 及以后版本的产品。
上一代 Product Manager（`product-manager`）只能安装 Terapines 4.x 产品，不属于此 skill 的适用范围。
这里的版本范围指被管理的产品版本，不是管理器自身的版本号。

Product Manager Next skill 会根据已安装 CLI 的帮助信息确认具体命令。
可执行的操作取决于你的产品版本和权限。

## 📦 安装

### 🤖 让 agent 安装

将下面这段话发给你的 agent：

```text
请阅读 https://raw.githubusercontent.com/Terapines/terapines-production-skills/main/INSTALL.md
并帮我安装 product-manager-next skill。
```

[Agent 安装指南](INSTALL.md) 包含下载来源、安装位置、操作步骤和验证方式。
具有本地文件访问权限的 agent 可以代为安装；仅能浏览网页的 agent 可以提供手动操作说明。
使用这种方式时，你不需要预先克隆仓库。

### 🛠️ Codex：手动安装

先克隆本仓库，或从 GitHub 下载并解压：

```bash
git clone https://github.com/Terapines/terapines-production-skills.git
cd terapines-production-skills
```

Codex 内置的 skill 安装器使用 `$CODEX_HOME/skills`，默认路径为
`~/.codex/skills`。在 Linux 上，从本仓库根目录执行：

```bash
skill_dir="${CODEX_HOME:-$HOME/.codex}/skills"
mkdir -p "$skill_dir"
if [ -e "$skill_dir/product-manager-next" ] || [ -L "$skill_dir/product-manager-next" ]; then
  echo "Skill already exists; review or back it up before replacing it."
else
  cp -R skills/product-manager-next "$skill_dir/"
fi
```

Windows 用户可将 `skills/product-manager-next` 复制到 `CODEX_HOME` 下的
`skills` 目录；未设置 CODEX_HOME 时，使用 `%USERPROFILE%\.codex\skills`。
安装后的入口文件应为 `skills/product-manager-next/SKILL.md`。

安装完成后，新建对话并使用 `$product-manager-next` 调用。
如果 skill 未出现在列表中，请重启 agent 并检查其 skill 发现配置。

### 🔌 其他 agent

对于支持 SKILL.md 的 agent，请按照该 agent 的 skill 安装说明，将完整的
`skills/product-manager-next` 目录复制到指定位置。
不同 agent 的发现路径和调用语法可能不同。

## 🚀 使用

安装 skill 不会自动安装 Product Manager Next、ZCC 或 LMD 服务。
如果尚未安装 Product Manager Next，可以让 agent 从官网获取安装器并协助安装。
如果已经安装，但 agent 无法找到 Product Manager Next，请提供可执行文件路径。

在 Codex 中可以这样提问：

```text
$product-manager-next 帮我从官网安装 Product Manager Next。

$product-manager-next 检查我的 Product Manager Next 版本和当前授权状态。

$product-manager-next 帮我安装 ZCC，先检查已有安装和当前 Product Manager Next 版本支持的选项。

$product-manager-next 帮我排查 Product Manager Next 为什么报告在线激活失败。
```

说明目标机器、产品及版本和期望结果。agent 可以先检查现有状态，再执行变更，
并识别哪些操作需要管理员权限。请将凭据和 license 文件保存在本仓库之外。

## 🔄 更新或卸载

更新时，获取所需的仓库版本，检查或备份本地修改后，再替换已安装的 skill 目录。
卸载时，仅删除该已安装的 skill 目录；这不会卸载 Terapines 产品或改变授权状态。
