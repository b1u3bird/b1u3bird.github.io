+++
title = "Windows 下的 clangd 配置记录"
date = 2026-01-27T09:00:00+08:00
tags = ["neovim", "clangd"]
+++

Windows 下配置 clangd 的关键是让编译参数和 include 路径稳定可复现。对于单文件算法竞赛项目，`compile_flags.txt` 通常比完整 CMake 更直接。

## 参数

保留标准版本、警告开关和 MinGW include 路径即可覆盖大多数补全场景。
