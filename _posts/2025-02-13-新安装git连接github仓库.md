---
layout: post
title: 新安装git连接github仓库
tags: git
---

在 Windows 系统上配置 Git 和 GitHub 的 SSH 传输与在其他系统上类似，但有一些特定的步骤和工具需要注意。以下是针对 Windows 系统的详细指南：

---

### **1. 安装 Git**

1. **下载 Git**:

   - 访问 [Git 官网](https://git-scm.com/) 下载 Windows 版本的 Git 安装包（`.exe` 文件）。

2. **安装 Git**:

   - 运行下载的安装程序。
   - 在安装过程中，按照默认选项即可，但需要注意以下几点：
     - 在 **"Select Components"** 页面，勾选 **"Git Bash Here"** 和 **"Git GUI Here"**。
     - 在 **"Choosing the default editor used by Git"** 页面，选择你喜欢的文本编辑器（如 **Notepad++** 或 **VSCode**）。
     - 在 **"Adjusting your PATH environment"** 页面，选择 **"Git from the command line and also from 3rd-party software"**。
     - 在 **"Configuring the line ending conversions"** 页面，选择 **"Checkout Windows-style, commit Unix-style line endings"**。
     - 其他选项保持默认即可。

3. **验证安装**:
   - 打开 **Git Bash**（在开始菜单中搜索）。
   - 输入以下命令，查看 Git 是否安装成功：
     ```bash
     git --version
     ```
   - 如果显示 Git 版本号（如 `git version 2.37.1`），说明安装成功。

---

### **2. 配置 Git 用户信息**

1. 打开 **Git Bash**。
2. 设置用户名和邮箱（Git 提交时会用到这些信息）：

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

   - 将 `"Your Name"` 替换为你的名字，`"your.email@example.com"` 替换为你的邮箱。

3. 验证配置：
   ```bash
   git config --list
   ```
   - 检查输出中是否有 `user.name` 和 `user.email`，确认配置是否正确。

---

### **3. 配置 GitHub 的 SSH 传输**

SSH 是一种安全的传输协议，可以让你在不输入密码的情况下与 GitHub 进行通信。

#### **步骤 1：检查是否已有 SSH 密钥**

1. 打开 **Git Bash**。
2. 输入以下命令检查是否已有 SSH 密钥：
   ```bash
   ls ~/.ssh
   ```
   - 如果看到 `id_rsa` 和 `id_rsa.pub` 文件，说明你已经有一个 SSH 密钥对。

#### **步骤 2：生成新的 SSH 密钥（如果没有）**

1. 生成 SSH 密钥：

   ```bash
   ssh-keygen -t rsa -b 4096 -C "your.email@example.com"
   ```

   - 将 `"your.email@example.com"` 替换为你的 GitHub 邮箱。
   - 按回车键接受默认文件路径（`~/.ssh/id_rsa`）。
   - 设置一个安全的密码（可选）。

2. 启动 SSH 代理：

   ```bash
   eval "$(ssh-agent -s)"
   ```

3. 将 SSH 私钥添加到 SSH 代理：
   ```bash
   ssh-add ~/.ssh/id_rsa
   ```

#### **步骤 3：将 SSH 公钥添加到 GitHub**

1. 复制 SSH 公钥：

   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

   - 复制输出的内容（以 `ssh-rsa` 开头，以你的邮箱结尾）。

2. 登录 GitHub：

   - 打开 [GitHub](https://github.com/) 并登录你的账户。

3. 添加 SSH 密钥：
   - 点击右上角头像，选择 **Settings**。
   - 在左侧菜单中选择 **SSH and GPG keys**。
   - 点击 **New SSH key**。
   - 在 **Title** 中输入一个描述（如 `My Windows Laptop`）。
   - 在 **Key** 中粘贴你复制的 SSH 公钥。
   - 点击 **Add SSH key**。

#### **步骤 4：测试 SSH 连接**

1. 在 **Git Bash** 中输入以下命令测试连接：
   ```bash
   ssh -T git@github.com
   ```
2. 如果看到以下提示，说明配置成功：
   ```
   Hi username! You've successfully authenticated, but GitHub does not provide shell access.
   ```

---

### **4. 使用 Git 和 GitHub**

1. **克隆仓库**：

   - 使用 SSH 克隆 GitHub 仓库：
     ```bash
     git clone git@github.com:username/repository.git
     ```
   - 将 `username/repository.git` 替换为你的 GitHub 仓库地址。

2. **提交更改**：
   - 修改文件后，使用以下命令提交更改：
     ```bash
     git add .
     git commit -m "Your commit message"
     git push origin main
     ```

---

### **5. 使用 HTTPS 代替 SSH**

如果你无法解决端口 22 被阻止的问题，可以暂时使用 HTTPS 进行 Git 操作。HTTPS 使用端口 443，通常不会被阻止。

GitHub 支持通过 HTTPS 端口（443）进行 SSH 连接。你可以通过修改 SSH 配置来实现。

#### **步骤**：

1. 打开或创建 SSH 配置文件：

   - 在 Git Bash 或终端中运行：
     ```bash
     nano ~/.ssh/config
     ```
   - 如果文件不存在，会自动创建。

2. 添加以下内容：

   ```
   Host github.com
     HostName ssh.github.com
     User git
     Port 443
     IdentityFile ~/.ssh/id_rsa
   ```

   - 保存并退出（按 `Ctrl + X`，然后按 `Y` 确认）。

3. 测试 SSH 连接：
   ```bash
   ssh -T git@github.com
   ```
   - 如果成功，你会看到以下提示：
     ```
     Hi username! You've successfully authenticated, but GitHub does not provide shell access.
     ```

---

### **6. 其他注意事项**

- **Git Bash** 是 Windows 上推荐使用的终端工具，它提供了类似 Linux 的命令行体验。
- 如果你更喜欢图形化工具，可以安装 **GitHub Desktop** 或 **Sourcetree**。

---

完成以上步骤后，你的 Git 和 GitHub 配置就完成了！
