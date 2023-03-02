# 快捷键开启Clash并开启系统代理

（声明：当前仅支持Windows系统）

首先，编写脚本(.bat)实现Clash的启动和开启系统代理

### Clash.bat

```bash
@echo off
start "" "D:\Desktop\Files\Clash.for.Windows-0.19.13-win\Clash for Windows.exe"
timeout /t 1 /nobreak > NUL
start "" "C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe" -ExecutionPolicy Bypass -WindowStyle Hidden -NoLogo -NoProfile -Command "Set-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings' -Name ProxyEnable -Value 1; Set-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings' -Name ProxyServer -Value '127.0.0.1:7890';"
exit
```

将第二行中Clash的路径改为你的路径

**那么，既然可以快捷启动，我们也需要快捷关闭**

### off.ps1

```powershell
Set-ItemProperty -Path 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings' -Name ProxyEnable -Value 0
```

### off.bat

```bash
powershell.exe -ExecutionPolicy Bypass -File "D:\Desktop\Projects\Tools\Clash\off.ps1"
```

将双引号内的路径改为你放置`off.ps1`的位置

off.bat实际上只是调用了`Powershell`去执行上面`off.ps1`中的指令

至此，我们已经实现了**双击Clash.bat打开代理，双击off.bat关闭代理**，但是这样依然不够优雅:)

### 设置快捷键

由于Windows对于一个程序的快捷键创建方式是先创建快捷方式，再给快捷方式赋予快捷键，这样快捷键的存在周期就是当前用户的登陆状态，也就是说，这个快捷键需要每次开机以后重启设置。

那么有没有一种方式，可以设定一个永久的快捷键，这里就需要引入第三方工具——[uTools官网 - 新一代效率工具平台](https://u.tools/)

下载`Utools`以后，在插件市场中搜索并安装“一步到位”，打开以后选择自己创建自动化脚本（一共两个，分别对应开关代理）

```javascript
if (utools.isWindows()) {
  // 打开 CMD 并执行脚本       
  require('child_process').spawn('start', ['cmd', '/k', 'start D:\\Desktop\\Projects\\Tools\\Clash\\Clash.bat'], { shell: 'cmd.exe', detached: true });
  }
```

替换为自己的路径并为该脚本设立关键字`Clash`

```javascript
if (utools.isWindows()) {
  // 打开 CMD 并执行脚本       
  require('child_process').spawn('start', ['cmd', '/k', 'start D:\\Desktop\\Projects\\Tools\\Clash\\off.bat'], { shell: 'cmd.exe', detached: true });
  }
```

替换为自己的路径并为该脚本设立关键字`Off`

我们来到了**最后一步**

打开`Utools`-偏好设置-全局快捷键，为上述两个脚本添加你顺手的快捷键，这样一来快捷键将会永久存在。