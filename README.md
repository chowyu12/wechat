# Mac 微信双开，支持 4.0 及以上版本

在终端中输入以下命令（注意空格和大小写）：

```bash
sudo cp -R /Applications/WeChat.app /Applications/微信双开.app
```

### **关键修正点：**

1. **删除多余的  `/Applications`**：原命令中的  `/Applications/WeChat.app/Applications`  会尝试复制不存在的子目录，正确路径应为  `/Applications/WeChat.app`。
2. **调整目标路径格式**：`/Applications/微信双开.app`  前的斜杠不能省略，否则会在当前目录（通常是用户主目录）下创建应用。

### **验证方法**

执行命令后，打开  **Finder > 应用程序**，如果看到  **微信双开.app**  图标（灰色或正常），说明克隆成功。若未出现，检查以下几点：

1. 是否正确输入  `WeChat`（区分大小写）。
2. 是否漏掉  `R`  参数（递归复制整个应用包）。
3. 终端是否提示权限不足（需输入管理员密码）。

### **后续步骤（完整流程）**

如果克隆成功，继续执行以下命令修改应用标识和签名：

```bash
# 修改应用唯一标识符（避免进程冲突）
sudo /usr/libexec/PlistBuddy -c "Set :CFBundleIdentifier net.maclub.wechat" /Applications/微信双开.app/Contents/Info.plist

# 重新签名应用（绕过系统验证）
sudo codesign --force --deep --sign - /Applications/微信双开.app
```

### **常见错误及解决**

1. **错误提示  `cp: /Applications/WeChat.app/Applications: No such file or directory`**→ 说明命令中多了  `/Applications`，使用修正后的命令。
2. **克隆后图标显示异常（灰色）**→ 这是正常现象，只要能正常打开即可。若无法打开，重新执行签名步骤。
3. **提示 “应用已损坏，移到废纸篓”**→ 执行  `sudo xattr -cr /Applications/微信双开.app`  后重试。
4. 版本升级后，需要删除重试上面操作（删除微信双开.app，聊天记录不会删除）。
