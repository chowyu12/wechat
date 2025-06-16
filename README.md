# Mac 微信双开，支持 4.0 及以上版本

（完整流程）在终端中输入以下命令（注意空格和大小写）：

```bash
# 复制应用
sudo cp -R /Applications/WeChat.app /Applications/微信双开.app
# 修改应用唯一标识符（避免进程冲突）
sudo /usr/libexec/PlistBuddy -c "Set :CFBundleIdentifier net.maclub.wechat" /Applications/微信双开.app/Contents/Info.plist

# 重新签名应用（绕过系统验证）
sudo codesign --force --deep --sign - /Applications/微信双开.app
```

### **验证方法**

执行命令后，打开  **Finder > 应用程序**，如果看到  **微信双开.app**  图标（灰色或正常），说明克隆成功。若未出现，检查以下几点：

1. 是否正确输入  `WeChat`（区分大小写）。
2. 是否漏掉  `R`  参数（递归复制整个应用包）。
3. 终端是否提示权限不足（需输入管理员密码）。

### **常见错误及解决**

1. **错误提示  `cp: /Applications/WeChat.app/Applications: No such file or directory`**→ 说明命令中多了  `/Applications`，使用修正后的命令。
2. **克隆后图标显示异常（灰色）**→ 这是正常现象，只要能正常打开即可。若无法打开，重新执行签名步骤。
3. **提示 “应用已损坏，移到废纸篓”**→ 执行  `sudo xattr -cr /Applications/微信双开.app`  后重试。
4. 版本升级后，需要删除重试上面操作（删除微信双开.app，聊天记录不会丢失）。
