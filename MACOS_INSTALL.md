# macOS 安装说明

## 问题：应用提示"已损坏，无法打开"

这是 macOS Gatekeeper 安全机制，因为应用未经过 Apple 代码签名。

## 解决方法

### 方法一：临时允许运行（适用于测试）

1. 下载并解压 `.zip` 文件
2. 打开终端，执行以下命令：

```bash
# 移除隔离属性
xattr -cr /Applications/electron-forge-github-action.app

# 或者使用下载路径
xattr -cr ~/Downloads/electron-forge-github-action.app
```

3. 现在可以正常打开应用

### 方法二：系统设置中允许

1. 尝试打开应用（会提示已损坏）
2. 打开 **系统设置** → **隐私与安全性**
3. 在底部找到被阻止的应用，点击 **仍要打开**

### 方法三：使用代码签名（推荐用于正式发布）

需要 Apple Developer 账号（$99/年）：

1. 安装依赖：
```bash
npm install --save-dev @electron/osx-sign @electron/notarize
```

2. 在 `forge.config.js` 中取消注释代码签名配置

3. 在 GitHub 仓库设置中添加 Secrets：
   - `APPLE_ID`: Apple ID 邮箱
   - `APPLE_APP_SPECIFIC_PASSWORD`: App 专用密码
   - `APPLE_TEAM_ID`: 开发者团队 ID

4. 在 `.github/workflows/build.yml` 中取消注释环境变量

5. 创建 `entitlements.plist` 文件（如需要）

## 为什么会这样？

- macOS 要求从互联网下载的应用必须经过 Apple 公证
- 未签名的应用会被 Gatekeeper 阻止
- 代码签名需要付费的 Apple Developer 账号
- 对于个人项目或内部使用，可以使用方法一临时绕过
