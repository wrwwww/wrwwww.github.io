---
title: win脚本执行错误处理
date: 2023-05-05 21:30:42
tags:  
---
---
title: win脚本执行错误处理
date: 2023-05-05 21:30:42
tags: [win,powershell]
---
```shell
PS D:\code\wrwwww.github.io> npm install -g pnpm

added 1 package in 11s

1 package is looking for funding
  run `npm fund` for details
PS D:\code\wrwwww.github.io> pnpm -v
pnpm : 无法加载文件 C:\Users\admin\AppData\Roaming\npm\pnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http
s:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ pnpm -v
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
PS D:\code\wrwwww.github.io> pnpm
pnpm : 无法加载文件 C:\Users\admin\AppData\Roaming\npm\pnpm.ps1，因为在此系统上禁止运行脚本。有关详细信息，请参阅 http
s:/go.microsoft.com/fwlink/?LinkID=135170 中的 about_Execution_Policies。
所在位置 行:1 字符: 1
+ pnpm
+ ~~~~
    + CategoryInfo          : SecurityError: (:) []，PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```
> 原因是： 首次在计算机上启动 Windows PowerShell 时，现用执行策略很可能是 Restricted（默认设置）。Restricted 策略不允许任何脚本运行。
解决办法:powershell使用管理员执行
```shell
set-ExecutionPolicy RemoteSigned
```