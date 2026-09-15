# WSL Ubuntu 安装 Claude Code \+ Windows CCSwitch 配置模型完整教程

## 一、环境说明

- 运行环境：Windows 11 \+ WSL2 Ubuntu

- Claude Code：**Ubuntu 原生二进制安装**（无 Node/npm 依赖、不占用Windows C盘、规避国内网络报错）

- 模型管理：Windows 端 CCSwitch（图形化切换大模型、配置API，无需操作Linux命令）

- 核心优势：彻底解决 npm WSL 路径Bug、不污染C盘、国内模型（DeepSeek等）无痛适配、支持Windows目录直接调用Claude Code

## 二、卸载旧冲突版本（前置清理）

若之前用npm安装过Claude Code，先彻底清理残留，避免冲突：

```bash
# 卸载npm版本claude-code
npm uninstall -g @anthropic-ai/claude-code

# 删除旧配置缓存
rm -rf ~/.claude ~/.npm ~/.npm-global
```

## 三、WSL Ubuntu 原生安装 Claude Code（核心步骤）

使用官方Linux二进制安装，**不依赖Node、不占用C盘**，适配国内网络：

```bash
# 官方一键安装脚本（国内可通，避开claude.ai登录校验）
curl -fsSL https://claude.ai/install.sh | bash
```

安装完成后，按提示写入环境变量（必须执行）：

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
```

### 3\.1 验证安装是否成功

```bash
# 查看版本
claude --version

# 查看安装路径（纯Linux路径，无C盘，正常）
which claude

# 完整环境检测（无报错即安装成功）
claude doctor
```

正常输出：`/home/lfr/.local/bin/claude`，安装类型为 `native`，无任何安装报错。

## 四、Windows 安装 CCSwitch

直接在Windows宿主机安装，**无需在Ubuntu装图形版**，图形化操作更便捷：

1. 前往 CC\-Switch 官方仓库 Releases 下载 Windows 版（msi/便携zip）

2. 正常安装/解压，打开 CCSwitch 软件

## 五、CCSwitch 对接 WSL Claude Code（关键配置）

### 5\.1 获取 WSL 配置目录路径

Ubuntu终端执行，获取Windows可识别的WSL路径：

```bash
wslpath -w ~/.claude
```

固定输出（你的专属路径）：`\\wsl.localhost\Ubuntu\home\lfr\.claude`

### 5\.2 CCSwitch 高级配置

1. 打开 CCSwitch → **设置 → 高级**

2. 找到「Claude Code 配置目录」，粘贴上述路径：`\\wsl.localhost\Ubuntu\home\lfr\.claude`

3. 保存设置，无需修改软件自身的C盘配置目录

4. 开启：应用到Claude Code插件配置接管、跳过初次安装确认

### 5\.3 配置自定义模型（以DeepSeek为例）

1. CCSwitch主界面 → 选择「Claude Code」→ 新增供应商

2. 填写接口信息（国内通用DeepSeek适配Claude接口）：
        

    - 接口地址：`https://api.deepseek.com/anthropic`

    - API Key：你的DeepSeek密钥（sk开头）

    - 默认模型：`deepseek-v4-flash / deepseek-v4-pro`

3. 填写完成后，点击**应用配置**

### 5\.4 验证模型配置生效

Ubuntu终端查看自动生成的配置文件：

```bash
cat ~/.claude/settings.json
```

文件存在且包含 `ANTHROPIC_BASE_URL`、`ANTHROPIC_AUTH_TOKEN`、模型字段，即配置成功。

## 六、Windows 文件夹使用 WSL 版 Claude Code（核心用法）

无需迁移代码、无需改配置，直接操作Windows本地项目文件。

### 6\.1 最快使用方式（推荐）

1. Windows资源管理器打开你的**项目文件夹**（C盘/D盘任意代码目录）

2. 文件夹顶部地址栏输入 `wsl` 回车

3. 自动弹出WSL终端，且当前目录直接定位到该Windows文件夹

4. 直接启动Claude Code：
        `claude`

### 6\.2 手动路径切换方式

Windows路径与WSL路径映射规则：

- Windows：`C:\Users\lfr\Desktop\项目文件夹` → WSL：`/mnt/c/Users/lfr/Desktop/项目文件夹`

- Windows：`D:\code\demo` → WSL：`/mnt/d/code/demo`

示例命令：

```bash
# 进入Windows桌面项目
cd /mnt/c/Users/lfr/Desktop/你的项目名
claude
```

### 6\.3 验证是否读取Windows文件

进入claude会话后，输入 `ls`，能列出Windows文件夹内所有文件即成功，可直接读写、修改、生成代码。

## 七、常用 Claude Code 会话指令

- `/model`：查看当前生效模型

- `/doctor`：项目完整环境检测

- `/clear`：清空当前会话

- `/exit`：退出Claude Code

## 八、关键注意事项

1. 本次安装为**WSL原生程序**，所有Claude文件存于Ubuntu虚拟磁盘，**不占用Windows C盘**

2. 全程无需访问claude\.ai官网，规避国内网络限制，完全依靠API Key\+CCSwitch工作

3. 后续切换模型、更换API，只需在Windows CCSwitch操作，一键应用无需改代码

4. Windows文件夹运行速度略慢于WSL本地目录，小项目完全够用，大项目建议放入WSL目录提速

> （注：部分内容可能由 AI 生成）
