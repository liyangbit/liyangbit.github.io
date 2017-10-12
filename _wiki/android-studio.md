---
layout: wiki
title: Android Studio
categories: Android
description: Android Studio 快捷键及使用技巧汇总
keywords: Android, Android Studio
---

## 快捷键

C --> Ctrl

S --> Shift

M --> Alt

Cmd --> Command

### 导航与跳转

| 功能                   | 快捷键 for win | 快捷键 for mac      |
|:-----------------------|:---------------|:--------------------|
| Find Action            | C-S-a          | Cmd-S-a             |
| 最近使用的文件         | C-e            | Cmd-e               |
| Outline                | C-F12          | Cmd-F12             |
| 定位到导航条           | M-Home         | Cmd-Up              |
| 快速打开类定义         | C-n            | Cmd-n               |
| 跳转至指定符号         | C-S-M-n        | Cmd-S-M-n           |
| 快速打开文件           | C-S-n          | Cmd-S-o             |
| 在...中选定当前文件    | M-F1           | M-F1                |
| 最近编辑过的文件       | C-S-e          | Cmd-S-e             |
| 打开光标所在变量类定义 | C-S-b          | Cmd-S-b             |
| 跳到变量/函数/类定义   | C-b/鼠标左键   | Cmd-b/鼠标左键/Down |
| 导航 Back/Forward      | C-M-Left/Right | Cmd-[/]             |
| Super Method           | C-u            | Cmd-u               |
| Implementations        | C-M-b          | Cmd-M-b             |
| Method Hierarchy       | C-S-h          | Cmd-S-h             |
| Type Hierarchy         | C-h            | C-h                 |
| Call Hierarchy         | C-M-h          | C-M-h               |
| Find Usages            | M-F7           | M-F7                |
| Show Usages            | C-M-F7         | Cmd-M-F7            |
| 搜索 Everywhere        | 双击 S         | 双击 S              |
| 搜索文本               | C-S-f          | Cmd-S-f             |
| 打开文件所在目录       | M-F1 8         | M-F1 8              |
| 打开设置               | C-M-s          | Cmd-,               |

### 切换至指定的工具窗口

| 功能     | 快捷键 for win | 快捷键 for mac |
|:---------|:---------------|:---------------|
| Project  | M-1            | Cmd-1          |
| Terminal | M-F12          | M-F12          |
| Editor   | Esc            | Esc            |

### 编辑

| 功能                    | 快捷键 for win | 快捷键 for mac      |
|:------------------------|:---------------|:--------------------|
| 注释/取消注释（//）     | C-/            | Cmd-/               |
| 注释/取消注释（/\*\*/） | C-S-/          | C-S-/               |
| 格式化代码              | C-M-l          | Cmd-M-l             |
| 智能 Import             | C-M-o          | C-M-o               |
| Undo                    | C-z            | Cmd-z               |
| Redo                    | C-S-z          | Cmd-S-z             |
| 删除行                  | C-y            | Cmd-x               |
| 复制行                  | C-c            | Cmd-c               |
| 复制当前行到下一行      | C-d            | Cmd-d               |
| 在下面另起一行          | S-Enter        | S-Enter             |
| 在上面另起一行          | C-M-Enter      | Cmd-M-Enter         |
| 上/下移动代码行         | M-S-Up/Down    | M-S-Up/Down         |
| 上/下一处引用           | C-M-Up/Down    | Cmd-M-Up/Down       |
| 关闭浮动窗口            | S-Esc          | S-Esc               |
| 单步                    | F8             | F8                  |
| 步入                    | F7             | F7                  |
| 继续执行                | F9             | Cmd-M-r             |
| 执行直到返回            | S-F8           | S-F8                |
| 执行到光标处            | M-F9           | M-F9                |
| 运行当前应用            | S-F10          | C-r                 |
| 调试当前应用            | S-F9           | C-d                 |
| 开/关断点               | C-F8/鼠标左键  | Cmd-F8              |
| 查看所有断点            | C-S-F8         | Cmd-S-F8            |
| 自动生成                | M-Insert       | Cmd-n               |
| 隐藏底部视图            | S-Esc          | S-Esc               |
| 删除一个单词            | C-Backspace    | M-delete            |
| 重构 - 重命名           | S-F6           | S-F6                |

注：

* 在 Windows 下格式化代码的 Ctrl+Alt+l 和 QQ 锁热键冲突了，我去 QQ 的设置里删除了QQ 锁热键。
* 在 Windows 下打开 Settings 的 Ctrl+Alt+s 与 QQ 打开消息盒子热键冲突了，我删除了 QQ 里的该热键。

## 插件

* [android-styler](https://github.com/alexzaitsev/android-styler)

  Android Studio / IDEA plugin that helps to create styles

## 打包

* [Gradle编译打包小结](http://blog.csdn.net/byhook/article/details/51746825)
* [Gradle构建Android项目](https://segmentfault.com/a/1190000002910311)

## 设置

此部分与 Intellj IDEA 通用。

### 在工程视图里隐藏指定类型文件

比如要隐藏 Vim 打开文件产生的 `*.swp` 文件，操作步骤：

1. 在 Android Studio 里打开 File - Settings - Editor - File Types；

2. 在 Ignore files and folders 里加上 `*.swp`，点 OK 保存。

### 屏蔽 IDE 的某些弹出通知

有些 IDE 通知经常弹出，而你却并不想看到它，比如 Usage Statistics，Vcs Notifications 等，屏蔽步骤：

1. 在 Android Studio 里打开 File - Settings - Appearance & Behavior - Notifications；

2. 在界面上找到你想屏蔽的 Group，将 Popup 列改为 No popup，点 OK 保存即可。

## 其它信息

### Android Plugin 与 Gradle 版本对应

<https://developer.android.com/studio/releases/gradle-plugin.html>
