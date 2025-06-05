#### 1. 安装```cliclick```工具
cliclick 是一个 macos 的命令行工具，用于模拟鼠标和键盘事件。
```
brew install cliclick
```
#### 2. 编写AppleScript脚本
```
-- 使用System Events模拟鼠标点击
tell application "System Events"
    -- 设置鼠标位置，这里的x和y需要根据实际游戏窗口中的位置调整
    set mouseX to 500 -- 设置鼠标的x坐标
    set mouseY to 300 -- 设置鼠标的y坐标

    -- 移动鼠标到指定位置
    do shell script "/usr/local/bin/cliclick m:" & mouseX & "," & mouseY

    -- 模拟鼠标点击
    do shell script "/usr/local/bin/cliclick c:" & mouseX & "," & mouseY
end tell
```